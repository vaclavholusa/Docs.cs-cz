---
title: "Hostitele ve službě Windows"
author: tdykstra
description: "Zjistěte, jak hostovat aplikace ASP.NET Core ve službě Windows."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/30/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: beda34dbd613f6ffe0afa207ab57dd6ebbc489ee
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="2bf29-103">Hostování aplikace ASP.NET Core ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="2bf29-103">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="2bf29-104">podle [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2bf29-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="2bf29-105">Doporučeným způsobem, jak hostovat aplikace ASP.NET Core v systému Windows bez použití služby IIS je chcete-li používat [služba systému Windows](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="2bf29-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="2bf29-106">Tímto způsobem ji může automaticky spustit po restartování počítače a dojde k chybě, bez čekání na uživatele k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="2bf29-106">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="2bf29-107">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2bf29-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="2bf29-108">Najdete v článku [další kroky](#next-steps) pokyny o tom, jak ji spustit.</span><span class="sxs-lookup"><span data-stu-id="2bf29-108">See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2bf29-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2bf29-109">Prerequisites</span></span>

* <span data-ttu-id="2bf29-110">Aplikace musíte spustit na modul runtime rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2bf29-110">The app must run on the .NET Framework runtime.</span></span>  <span data-ttu-id="2bf29-111">V *.csproj* souboru, zadejte odpovídající hodnoty pro [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) a [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="2bf29-111">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="2bf29-112">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="2bf29-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="2bf29-113">Při vytváření projektu v sadě Visual Studio, použijte **aplikace ASP.NET Core (rozhraní .NET Framework)** šablony.</span><span class="sxs-lookup"><span data-stu-id="2bf29-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="2bf29-114">Pokud aplikace přijímá požadavky od Internetu (nikoli pouze z interní sítě), musí používat [WebListener](xref:fundamentals/servers/weblistener) webový server místo [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="2bf29-114">If the app receives requests from the Internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="2bf29-115">Kestrel musí použít se službou IIS pro nasazení okraj.</span><span class="sxs-lookup"><span data-stu-id="2bf29-115">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="2bf29-116">Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="2bf29-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="2bf29-117">Začínáme</span><span class="sxs-lookup"><span data-stu-id="2bf29-117">Getting started</span></span>

<span data-ttu-id="2bf29-118">Tato část popisuje minimální změny, které jsou nezbytné k nastavení existujícího projektu ASP.NET Core ke spuštění ve službě.</span><span class="sxs-lookup"><span data-stu-id="2bf29-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="2bf29-119">Nainstalujte balíček NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="2bf29-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="2bf29-120">Proveďte následující změny v `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="2bf29-120">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="2bf29-121">Volání `host.RunAsService` místo `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="2bf29-121">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="2bf29-122">Pokud kód volá `UseContentRoot`, použijte cestu k umístění pro publikování místo`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="2bf29-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="2bf29-123">Publikování aplikace do složky.</span><span class="sxs-lookup"><span data-stu-id="2bf29-123">Publish the application to a folder.</span></span>

  <span data-ttu-id="2bf29-124">Použití [dotnet publikování](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) nebo [profil publikování se Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) který publikuje do složky.</span><span class="sxs-lookup"><span data-stu-id="2bf29-124">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

* <span data-ttu-id="2bf29-125">Otestujte vytváření a spouštění služby.</span><span class="sxs-lookup"><span data-stu-id="2bf29-125">Test by creating and starting the service.</span></span>

  <span data-ttu-id="2bf29-126">Otevřete okno příkazového řádku správce používat [sc.exe](https://technet.microsoft.com/library/bb490995) nástroj příkazového řádku k vytvoření a spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="2bf29-126">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="2bf29-127">Pokud je služba Moje_služba, publikujte aplikaci `c:\svc`a aplikace jmenuje AspNetCoreService, příkazy bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="2bf29-127">If the service is named MyService, publish the app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```

  <span data-ttu-id="2bf29-128">`binPath` Hodnota je cesta ke spustitelnému souboru aplikace, včetně názvu spustitelného souboru, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="2bf29-128">The `binPath` value is the path to the app's executable, including the executable filename itself.</span></span>

  ![Okno konzoly vytvořte a spusťte příklad](windows-service/_static/create-start.png)

  <span data-ttu-id="2bf29-130">Když se tyto příkazy dokončit, přejděte do stejnou cestu jako při spuštění jako konzolové aplikace (ve výchozím nastavení, `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="2bf29-130">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`)</span></span>

  ![Spuštěná ve službě](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="2bf29-132">Zadejte způsob, jak spustit mimo službu</span><span class="sxs-lookup"><span data-stu-id="2bf29-132">Provide a way to run outside of a service</span></span>

<span data-ttu-id="2bf29-133">Je snazší testování a ladění při spuštění mimo službu, takže se obvykle přidávat kód, který volá `host.RunAsService` pouze za určitých podmínek.</span><span class="sxs-lookup"><span data-stu-id="2bf29-133">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="2bf29-134">Například můžete spouštět aplikace jako konzolové aplikace s `--console` argument příkazového řádku nebo pokud je připojen ladicí program.</span><span class="sxs-lookup"><span data-stu-id="2bf29-134">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="2bf29-135">Zpracování ukončení a spuštění události</span><span class="sxs-lookup"><span data-stu-id="2bf29-135">Handle stopping and starting events</span></span>

<span data-ttu-id="2bf29-136">Pro zpracování `OnStarting`, `OnStarted`, a `OnStopping` události, proveďte následující další změny:</span><span class="sxs-lookup"><span data-stu-id="2bf29-136">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="2bf29-137">Vytvořte třídu, která je odvozena z `WebHostService`.</span><span class="sxs-lookup"><span data-stu-id="2bf29-137">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="2bf29-138">Vytvoření metody rozšíření pro `IWebHost` , předá vlastní `WebHostService` k `ServiceBase.Run`.</span><span class="sxs-lookup"><span data-stu-id="2bf29-138">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="2bf29-139">V `Program.Main` změnu volání nové metody rozšíření místo `host.RunAsService`.</span><span class="sxs-lookup"><span data-stu-id="2bf29-139">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="2bf29-140">Pokud vlastní `WebHostService` kódu je potřeba získat službu z vkládání závislostí (například protokolovač), získat z `Services` vlastnost `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="2bf29-140">If the custom `WebHostService` code needs to get a service from dependency injection (such as a logger), get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="2bf29-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2bf29-141">Next steps</span></span>

<span data-ttu-id="2bf29-142">[Ukázkové aplikace](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) Tento doprovodný článek je jednoduché webové aplikace MVC, která se změnila, jak je znázorněno v předchozích příklady kódu.</span><span class="sxs-lookup"><span data-stu-id="2bf29-142">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="2bf29-143">Chcete-li používat službu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2bf29-143">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="2bf29-144">Publikovat do *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="2bf29-144">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="2bf29-145">Otevřete okno správce.</span><span class="sxs-lookup"><span data-stu-id="2bf29-145">Open an administrator window.</span></span>

* <span data-ttu-id="2bf29-146">Zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="2bf29-146">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="2bf29-147">V prohlížeči přejděte na http://localhost: 5000 ověřit, zda je spuštěna.</span><span class="sxs-lookup"><span data-stu-id="2bf29-147">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="2bf29-148">Pokud aplikace nelze spustit očekávaným způsobem, při spuštění ve službě, je rychlý způsob, jak zpřístupnit chybové zprávy pro přidání poskytovatele protokolování, jako [zprostředkovatele protokolu událostí systému Windows](xref:fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="2bf29-148">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging/index#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="2bf29-149">Potvrzování</span><span class="sxs-lookup"><span data-stu-id="2bf29-149">Acknowledgments</span></span>

<span data-ttu-id="2bf29-150">Tento článek byl napsán pomocí zdroje, které již byly publikovány.</span><span class="sxs-lookup"><span data-stu-id="2bf29-150">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="2bf29-151">Nejdřívější a nejvhodnější z nich byly tyto:</span><span class="sxs-lookup"><span data-stu-id="2bf29-151">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="2bf29-152">Hostování ASP.NET Core jako služby systému Windows</span><span class="sxs-lookup"><span data-stu-id="2bf29-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="2bf29-153">Jak hostovat vaše základní technologie ASP.NET ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="2bf29-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
