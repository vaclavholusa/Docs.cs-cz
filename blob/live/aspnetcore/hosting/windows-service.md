---
title: "Hostitele ve službě Windows"
author: tdykstra
description: "Zjistěte, jak hostovat aplikace ASP.NET Core ve službě Windows."
keywords: "Hostování ASP.NET Core, služba systému Windows"
ms.author: tdykstra
manager: wpickett
ms.date: 03/30/2017
ms.topic: article
ms.assetid: d9a65066-d7cb-47df-b046-64629c4d2c6f
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/windows-service
ms.openlocfilehash: a6d1acf5ab8f40b0b4d487a6f34cd83d13907852
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="5d994-104">Hostování aplikace ASP.NET Core ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="5d994-104">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="5d994-105">podle [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5d994-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="5d994-106">Doporučený způsob k hostování aplikace ASP.NET Core v systému Windows, když nepoužíváte služby IIS je chcete-li používat [služba systému Windows](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="5d994-106">The recommended way to host an ASP.NET Core app on Windows when you don't use IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="5d994-107">Tímto způsobem ji může automaticky spustit po restartování počítače a dojde k chybě, bez čekání na uživatele k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="5d994-107">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="5d994-108">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="5d994-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="5d994-109">Najdete v článku [další kroky](#next-steps) pokyny o tom, jak ji spustit.</span><span class="sxs-lookup"><span data-stu-id="5d994-109">See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5d994-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5d994-110">Prerequisites</span></span>

* <span data-ttu-id="5d994-111">Aplikace musíte spustit na modul runtime rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5d994-111">The app must run on the .NET Framework runtime.</span></span>  <span data-ttu-id="5d994-112">V *.csproj* souboru, zadejte odpovídající hodnoty pro [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) a [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="5d994-112">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="5d994-113">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="5d994-113">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="5d994-114">Při vytváření projektu v sadě Visual Studio, použijte **aplikace ASP.NET Core (rozhraní .NET Framework)** šablony.</span><span class="sxs-lookup"><span data-stu-id="5d994-114">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="5d994-115">Pokud aplikace se zobrazí požadavky z Internetu (nikoli pouze z interní sítě), musíte použít [WebListener](xref:fundamentals/servers/weblistener) webový server místo [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="5d994-115">If the app will get requests from the internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="5d994-116">Kestrel musí použít se službou IIS pro nasazení okraj.</span><span class="sxs-lookup"><span data-stu-id="5d994-116">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="5d994-117">Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="5d994-117">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="5d994-118">Začínáme</span><span class="sxs-lookup"><span data-stu-id="5d994-118">Getting started</span></span>

<span data-ttu-id="5d994-119">Tato část popisuje minimální změny, které jsou nezbytné k nastavení existujícího projektu ASP.NET Core ke spuštění ve službě.</span><span class="sxs-lookup"><span data-stu-id="5d994-119">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="5d994-120">Nainstalujte balíček NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="5d994-120">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="5d994-121">Proveďte následující změny v `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="5d994-121">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="5d994-122">Volání `host.RunAsService` místo `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="5d994-122">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="5d994-123">Pokud váš kód volá `UseContentRoot`, použijte cestu k umístění pro publikování místo`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="5d994-123">If your code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="5d994-124">Publikování aplikace do složky.</span><span class="sxs-lookup"><span data-stu-id="5d994-124">Publish the application to a folder.</span></span>

  <span data-ttu-id="5d994-125">Použití [dotnet publikování](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) nebo [profil publikování se Visual Studio](xref:publishing/web-publishing-vs) který publikuje do složky.</span><span class="sxs-lookup"><span data-stu-id="5d994-125">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:publishing/web-publishing-vs) that publishes to a folder.</span></span>

* <span data-ttu-id="5d994-126">Otestujte vytváření a spouštění služby.</span><span class="sxs-lookup"><span data-stu-id="5d994-126">Test by creating and starting the service.</span></span>

  <span data-ttu-id="5d994-127">Otevřete okno příkazového řádku správce používat [sc.exe](https://technet.microsoft.com/library/bb490995) nástroj příkazového řádku k vytvoření a spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="5d994-127">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="5d994-128">Pokud zadáte název vaší služby Moje_služba, můžete publikovat aplikaci, kterou chcete `c:\svc`a aplikace jmenuje AspNetCoreService, příkazy bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="5d994-128">If you name your service MyService, you publish your app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  <span data-ttu-id="5d994-129">`binPath` Hodnota je cesta ke spustitelnému souboru vaší aplikace, včetně názvu spustitelného souboru, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="5d994-129">The `binPath` value is the path to your app's executable, including the executable filename itself.</span></span>

  ![Okno konzoly vytvořte a spusťte příklad](windows-service/_static/create-start.png)

  <span data-ttu-id="5d994-131">Po dokončení těchto příkazů, můžete vyhledat stejnou cestu jako při spuštění jako konzolové aplikace (ve výchozím nastavení, `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="5d994-131">When these commands finish, you can browse to the same path as when you run as a console app (by default, `http://localhost:5000`)</span></span>

  ![Spuštěná ve službě](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="5d994-133">Zadejte způsob, jak spustit mimo službu</span><span class="sxs-lookup"><span data-stu-id="5d994-133">Provide a way to run outside of a service</span></span>

<span data-ttu-id="5d994-134">Je snazší testování a ladění při používáte mimo službu, takže se obvykle přidávat kód, který volá `host.RunAsService` pouze za určitých podmínek.</span><span class="sxs-lookup"><span data-stu-id="5d994-134">It's easier to test and debug when you're running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="5d994-135">Například můžete spustit jako konzolovou aplikaci pokud dojde `--console` argument příkazového řádku nebo pokud je připojen ladicí program.</span><span class="sxs-lookup"><span data-stu-id="5d994-135">For example, you could run as a console app if you get a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="5d994-136">Zpracování ukončení a spuštění události</span><span class="sxs-lookup"><span data-stu-id="5d994-136">Handle stopping and starting events</span></span>

<span data-ttu-id="5d994-137">Pokud chcete zpracovávat `OnStarting`, `OnStarted`, a `OnStopping` události, proveďte následující další změny:</span><span class="sxs-lookup"><span data-stu-id="5d994-137">If you want to handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="5d994-138">Vytvořte třídu, která je odvozena z `WebHostService`.</span><span class="sxs-lookup"><span data-stu-id="5d994-138">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="5d994-139">Vytvoření metody rozšíření pro `IWebHost` , předá vaše vlastní `WebHostService` k `ServiceBase.Run`.</span><span class="sxs-lookup"><span data-stu-id="5d994-139">Create an extension method for `IWebHost` that passes your custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="5d994-140">V `Program.Main` změnu volání nové metody rozšíření místo `host.RunAsService`.</span><span class="sxs-lookup"><span data-stu-id="5d994-140">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="5d994-141">Pokud vaše vlastní `WebHostService` kódu je potřeba získat službu z vkládání závislostí (například protokolovač), můžete získat z `Services` vlastnost `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="5d994-141">If your custom `WebHostService` code needs to get a service from dependency injection (such as a logger), you can get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="5d994-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5d994-142">Next steps</span></span>

<span data-ttu-id="5d994-143">[Ukázkové aplikace](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) Tento doprovodný článek je jednoduché webové aplikace MVC, která se změnila, jak je znázorněno v předchozích příklady kódu.</span><span class="sxs-lookup"><span data-stu-id="5d994-143">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="5d994-144">Chcete-li používat službu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5d994-144">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="5d994-145">Publikovat do *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="5d994-145">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="5d994-146">Otevřete okno správce.</span><span class="sxs-lookup"><span data-stu-id="5d994-146">Open an administrator window.</span></span>

* <span data-ttu-id="5d994-147">Zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="5d994-147">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="5d994-148">V prohlížeči přejděte na http://localhost: 5000 ověřit, zda je spuštěna.</span><span class="sxs-lookup"><span data-stu-id="5d994-148">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="5d994-149">Pokud aplikace nelze spustit očekávaným způsobem, při spuštění ve službě, je rychlý způsob, jak zpřístupnit chybové zprávy pro přidání poskytovatele protokolování, jako [zprostředkovatele protokolu událostí systému Windows](xref:fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="5d994-149">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging/index#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="5d994-150">Potvrzování</span><span class="sxs-lookup"><span data-stu-id="5d994-150">Acknowledgments</span></span>

<span data-ttu-id="5d994-151">Tento článek byl napsán pomocí zdroje, které již byly publikovány.</span><span class="sxs-lookup"><span data-stu-id="5d994-151">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="5d994-152">Nejdřívější a nejvhodnější z nich byly tyto:</span><span class="sxs-lookup"><span data-stu-id="5d994-152">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="5d994-153">Hostování ASP.NET Core jako služby systému Windows</span><span class="sxs-lookup"><span data-stu-id="5d994-153">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="5d994-154">Jak hostovat vaše základní technologie ASP.NET ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="5d994-154">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
