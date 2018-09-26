---
title: Hostitele ASP.NET Core ve službě Windows
author: guardrex
description: Zjistěte, jak hostovat aplikace ASP.NET Core ve službě Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/25/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: eb88b0bb2e9ce4cfd3a7db2081ad7d62d5dcb08e
ms.sourcegitcommit: 599ebae5c2d6fcb22dfa6ae7d1f4bdfcacb79af4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/26/2018
ms.locfileid: "47211036"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="1f192-103">Hostitele ASP.NET Core ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="1f192-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="1f192-104">Podle [Luke Latham](https://github.com/guardrex) a [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="1f192-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="1f192-105">Aplikace ASP.NET Core je možné hostovat na Windows bez použití služby IIS jako [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="1f192-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="1f192-106">Pokud hostovaný jako služba Windows, aplikace může automaticky spuštění po restartování a dojde k chybě, které nevyžaduje zásah člověka.</span><span class="sxs-lookup"><span data-stu-id="1f192-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="1f192-107">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1f192-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="1f192-108">Převést projekt do služby Windows</span><span class="sxs-lookup"><span data-stu-id="1f192-108">Convert a project into a Windows Service</span></span>

<span data-ttu-id="1f192-109">Následující minimální změny je potřeba nastavit stávající projekt ASP.NET Core pro spouštěn jako služba:</span><span class="sxs-lookup"><span data-stu-id="1f192-109">The following minimum changes are required to set up an existing ASP.NET Core project to run as a service:</span></span>

1. <span data-ttu-id="1f192-110">V souboru projektu:</span><span class="sxs-lookup"><span data-stu-id="1f192-110">In the project file:</span></span>

   * <span data-ttu-id="1f192-111">Ověřte existenci Windows [identifikátor modulu Runtime (RID)](/dotnet/core/rid-catalog) nebo ho přidat do `<PropertyGroup>` , který obsahuje Cílová architektura:</span><span class="sxs-lookup"><span data-stu-id="1f192-111">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add it to the `<PropertyGroup>` that contains the target framework:</span></span>

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

      <span data-ttu-id="1f192-112">Chcete-li publikovat pro více identifikátorů RID:</span><span class="sxs-lookup"><span data-stu-id="1f192-112">To publish for multiple RIDs:</span></span>

      * <span data-ttu-id="1f192-113">Zadejte identifikátory RID v seznam oddělený středníkem.</span><span class="sxs-lookup"><span data-stu-id="1f192-113">Provide the RIDs in a semicolon-delimited list.</span></span>
      * <span data-ttu-id="1f192-114">Použijte název vlastnosti `<RuntimeIdentifiers>` (množné číslo).</span><span class="sxs-lookup"><span data-stu-id="1f192-114">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

      <span data-ttu-id="1f192-115">Další informace najdete v tématu [katalog identifikátorů RID .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="1f192-115">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="1f192-116">Přidat odkaz na balíček pro [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="1f192-116">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

1. <span data-ttu-id="1f192-117">Proveďte následující změny v `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="1f192-117">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="1f192-118">Volání [hostitele. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) místo `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="1f192-118">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="1f192-119">Volání [UseContentRoot](xref:fundamentals/host/web-host#content-root) a cesta k aplikaci pro publikování umístění, namísto použití `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="1f192-119">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="1f192-120">Publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f192-120">Publish the app.</span></span> <span data-ttu-id="1f192-121">Použití [dotnet publikovat](/dotnet/articles/core/tools/dotnet-publish) nebo [profil publikování pro Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="1f192-121">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span> <span data-ttu-id="1f192-122">Při používání sady Visual Studio, vyberte **FolderProfile**.</span><span class="sxs-lookup"><span data-stu-id="1f192-122">When using a Visual Studio, select the **FolderProfile**.</span></span>

   <span data-ttu-id="1f192-123">Chcete-li publikovat ukázkovou aplikaci pomocí nástrojů rozhraní příkazového řádku (CLI), spusťte [dotnet publikovat](/dotnet/core/tools/dotnet-publish) příkazu na příkazovém řádku ve složce projektu.</span><span class="sxs-lookup"><span data-stu-id="1f192-123">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder.</span></span> <span data-ttu-id="1f192-124">V musí být zadán identifikátor RID `<RuntimeIdenfifier>` (nebo `<RuntimeIdentifiers>`) vlastnost souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="1f192-124">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="1f192-125">V následujícím příkladu, publikování aplikace v rámci konfigurace verze pro `win7-x64` modulu runtime:</span><span class="sxs-lookup"><span data-stu-id="1f192-125">In the following example, the app is published in Release configuration for the `win7-x64` runtime:</span></span>

   ```console
   dotnet publish --configuration Release --runtime win7-x64
   ```

1. <span data-ttu-id="1f192-126">Použití [sc.exe](https://technet.microsoft.com/library/bb490995) nástroj příkazového řádku vytvořte službu.</span><span class="sxs-lookup"><span data-stu-id="1f192-126">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="1f192-127">`binPath` Hodnota je cesta ke spustitelnému souboru aplikace, která zahrnuje název spustitelného souboru.</span><span class="sxs-lookup"><span data-stu-id="1f192-127">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="1f192-128">**Mezera mezi znaménko rovná se a znak uvozovek na začátek cesty je povinný.**</span><span class="sxs-lookup"><span data-stu-id="1f192-128">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="1f192-129">Služby se publikují do složky projektu, použijte cestu k *publikovat* složku pro vytvoření služby.</span><span class="sxs-lookup"><span data-stu-id="1f192-129">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="1f192-130">V následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="1f192-130">In the following example:</span></span>

   * <span data-ttu-id="1f192-131">Projekt se nachází v *c:\\my_services\\AspNetCoreService* složky.</span><span class="sxs-lookup"><span data-stu-id="1f192-131">The project resides in the *c:\\my_services\\AspNetCoreService* folder.</span></span>
   * <span data-ttu-id="1f192-132">Projekt, se publikují v `Release` konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1f192-132">The project is published in `Release` configuration.</span></span>
   * <span data-ttu-id="1f192-133">Moniker cílového rozhraní (TFM) je `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="1f192-133">The Target Framework Moniker (TFM) is `netcoreapp2.1`.</span></span>
   * <span data-ttu-id="1f192-134">Identifikátor modulu Runtime (RID) je `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="1f192-134">The Runtime Identifer (RID) is `win7-x64`.</span></span>
   * <span data-ttu-id="1f192-135">Je název spustitelné aplikace *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="1f192-135">The app executable is named *AspNetCoreService.exe*.</span></span>
   * <span data-ttu-id="1f192-136">Služba má název **Moje_služba**.</span><span class="sxs-lookup"><span data-stu-id="1f192-136">The service is named **MyService**.</span></span>

   <span data-ttu-id="1f192-137">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1f192-137">Example:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="1f192-138">Ujistěte se, že je k dispozici mezi prostor `binPath=` argument a její hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1f192-138">Make sure the space is present between the `binPath=` argument and its value.</span></span>

   <span data-ttu-id="1f192-139">Publikovat a spustit službu z jiné složky:</span><span class="sxs-lookup"><span data-stu-id="1f192-139">To publish and start the service from a different folder:</span></span>

      * <span data-ttu-id="1f192-140">Použití [– výstupní &lt;OUTPUT_DIRECTORY&gt; ](/dotnet/core/tools/dotnet-publish#options) možnost `dotnet publish` příkazu.</span><span class="sxs-lookup"><span data-stu-id="1f192-140">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span> <span data-ttu-id="1f192-141">Pokud používáte Visual Studio, nakonfigurujte **cílové umístění** v **FolderProfile** publikovat stránku vlastností před výběrem **publikovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1f192-141">If using Visual Studio, configure the **Target Location** in the **FolderProfile** publish property page before selecting the **Publish** button.</span></span>
      * <span data-ttu-id="1f192-142">Vytvoření služby s `sc.exe` příkazu cesta k výstupní složce.</span><span class="sxs-lookup"><span data-stu-id="1f192-142">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="1f192-143">Zahrnout název spustitelného souboru služby na cestě k dispozici na `binPath`.</span><span class="sxs-lookup"><span data-stu-id="1f192-143">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="1f192-144">Spusťte službu pomocí `sc start <SERVICE_NAME>` příkazu.</span><span class="sxs-lookup"><span data-stu-id="1f192-144">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="1f192-145">Spustit službu ukázkové aplikace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1f192-145">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="1f192-146">Příkaz trvá několik sekund se spustit službu.</span><span class="sxs-lookup"><span data-stu-id="1f192-146">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="1f192-147">Chcete-li zkontrolovat stav služby, použijte `sc query <SERVICE_NAME>` příkazu.</span><span class="sxs-lookup"><span data-stu-id="1f192-147">To check the status of the service, use the `sc query <SERVICE_NAME>` command.</span></span> <span data-ttu-id="1f192-148">Stav je uveden jako jeden z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="1f192-148">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="1f192-149">Použijte následující příkaz a zkontrolujte stav služby app service vzorku:</span><span class="sxs-lookup"><span data-stu-id="1f192-149">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="1f192-150">Pokud je služba v `RUNNING` stavu a pokud je služba webové aplikace, procházet aplikace, její cesta (ve výchozím nastavení, `http://localhost:5000`, který přesměruje `https://localhost:5001` při použití [HTTPS přesměrování Middleware](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="1f192-150">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="1f192-151">Aplikační služba ukázkového procházet aplikace na adrese `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1f192-151">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="1f192-152">Zastavit službu s `sc stop <SERVICE_NAME>` příkazu.</span><span class="sxs-lookup"><span data-stu-id="1f192-152">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="1f192-153">Následující příkaz zastaví aplikační služba ukázkového:</span><span class="sxs-lookup"><span data-stu-id="1f192-153">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="1f192-154">Po krátké prodlevě zastavit službu, odinstalujte službu s `sc delete <SERVICE_NAME>` příkazu.</span><span class="sxs-lookup"><span data-stu-id="1f192-154">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="1f192-155">Postup kontroly stavu aplikační služba ukázkového:</span><span class="sxs-lookup"><span data-stu-id="1f192-155">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="1f192-156">Pokud je aplikační služba ukázkového v `STOPPED` stavu, použijte následující příkaz pro odinstalaci aplikační služba ukázkového:</span><span class="sxs-lookup"><span data-stu-id="1f192-156">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a><span data-ttu-id="1f192-157">Spuštění aplikace mimo službu</span><span class="sxs-lookup"><span data-stu-id="1f192-157">Run the app outside of a service</span></span>

<span data-ttu-id="1f192-158">Usnadňuje testování a ladění, když se provozují mimo službu, takže je obvyklý přidáte kód, který volá `RunAsService` pouze za určitých podmínek.</span><span class="sxs-lookup"><span data-stu-id="1f192-158">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="1f192-159">Například aplikace může běžet jako aplikace konzoly v jazyce `--console` argument příkazového řádku nebo pokud je připojen ladicí program:</span><span class="sxs-lookup"><span data-stu-id="1f192-159">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="1f192-160">Vzhledem k tomu, že konfigurace ASP.NET Core vyžaduje páry název hodnota pro argumenty příkazového řádku, `--console` přepínač byl předtím, než jsou předány argumenty [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="1f192-160">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="1f192-161">`isService` nebude zadán, využije z `Main` do `CreateWebHostBuilder` protože podpis `CreateWebHostBuilder` musí být `CreateWebHostBuilder(string[])` mohl [testování integrace](xref:test/integration-tests) fungovala správně.</span><span class="sxs-lookup"><span data-stu-id="1f192-161">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="1f192-162">Zpracování, spouštění a zastavování události</span><span class="sxs-lookup"><span data-stu-id="1f192-162">Handle stopping and starting events</span></span>

<span data-ttu-id="1f192-163">Pro zpracování [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), a [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) události, proveďte následující další změny:</span><span class="sxs-lookup"><span data-stu-id="1f192-163">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="1f192-164">Vytvořte třídu, která je odvozena z [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="1f192-164">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="1f192-165">Vytvořit metodu rozšíření pro [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) , který předá vlastní `WebHostService` k [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="1f192-165">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="1f192-166">V `Program.Main`, zavolat novou metodu rozšíření `RunAsCustomService`, namísto [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="1f192-166">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="1f192-167">`isService` nebude zadán, využije z `Main` do `CreateWebHostBuilder` protože podpis `CreateWebHostBuilder` musí být `CreateWebHostBuilder(string[])` mohl [testování integrace](xref:test/integration-tests) fungovala správně.</span><span class="sxs-lookup"><span data-stu-id="1f192-167">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="1f192-168">Pokud vlastní `WebHostService` kód vyžaduje službu z injektáž závislostí (jako je například protokolování), získat ze [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) vlastnost:</span><span class="sxs-lookup"><span data-stu-id="1f192-168">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="1f192-169">Proxy server a scénáře pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1f192-169">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="1f192-170">Služby, které interakci s žádostí z Internetu nebo podnikové síti a jsou za proxy nebo nástroj pro vyrovnávání zatížení může vyžadovat dodatečnou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="1f192-170">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="1f192-171">Další informace naleznete v tématu <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="1f192-171">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="1f192-172">Konfigurace HTTPS</span><span class="sxs-lookup"><span data-stu-id="1f192-172">Configure HTTPS</span></span>

<span data-ttu-id="1f192-173">Zadejte [konfigurace koncového bodu HTTPS serveru Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="1f192-173">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="1f192-174">Aktuální adresář a obsahu root</span><span class="sxs-lookup"><span data-stu-id="1f192-174">Current directory and content root</span></span>

<span data-ttu-id="1f192-175">Aktuální pracovní adresář vrátit voláním `Directory.GetCurrentDirectory()` pro službu Windows je *C:\\WINDOWS\\system32* složky.</span><span class="sxs-lookup"><span data-stu-id="1f192-175">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="1f192-176">*System32* složka není vhodné umístění pro ukládání souborů služby (například soubory nastavení).</span><span class="sxs-lookup"><span data-stu-id="1f192-176">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="1f192-177">Použijte jednu z následujících dvou přístupů k zajištění údržby a přístup k prostředky a nastavení souborů pomocí služby [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) při použití [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="1f192-177">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="1f192-178">Pomocí obsahu kořenovou cestu.</span><span class="sxs-lookup"><span data-stu-id="1f192-178">Use the content root path.</span></span> <span data-ttu-id="1f192-179">`IHostingEnvironment.ContentRootPath` Se stejnou cestu k dispozici na `binPath` argument při vytvoření služby.</span><span class="sxs-lookup"><span data-stu-id="1f192-179">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="1f192-180">Namísto použití `Directory.GetCurrentDirectory()` k vytvoření cesty k souborům nastavení, použijte obsah kořenové cestě a udržovat soubory v kořenové obsahu aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f192-180">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="1f192-181">Store soubory na vhodné místo na disku.</span><span class="sxs-lookup"><span data-stu-id="1f192-181">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="1f192-182">Zadejte absolutní cestu s `SetBasePath` do složky obsahující soubory.</span><span class="sxs-lookup"><span data-stu-id="1f192-182">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1f192-183">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1f192-183">Additional resources</span></span>

* <span data-ttu-id="1f192-184">[Konfigurace koncového bodu kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (včetně konfigurace protokolu HTTPS a podporu SNI)</span><span class="sxs-lookup"><span data-stu-id="1f192-184">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
