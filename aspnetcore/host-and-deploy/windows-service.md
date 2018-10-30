---
title: Hostitele ASP.NET Core ve službě Windows
author: guardrex
description: Zjistěte, jak hostovat aplikace ASP.NET Core ve službě Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/25/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 6e8e3cdc9d40ebe00fb8b78107c585e57e9e7c73
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244772"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="56f0a-103">Hostitele ASP.NET Core ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="56f0a-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="56f0a-104">Podle [Luke Latham](https://github.com/guardrex) a [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="56f0a-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="56f0a-105">Aplikace ASP.NET Core je možné hostovat na Windows bez použití služby IIS jako [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="56f0a-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="56f0a-106">Pokud hostovaný jako služba Windows, aplikace se automaticky spustí po restartování počítače.</span><span class="sxs-lookup"><span data-stu-id="56f0a-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="56f0a-107">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([stažení](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="56f0a-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="56f0a-108">Převést projekt do služby Windows</span><span class="sxs-lookup"><span data-stu-id="56f0a-108">Convert a project into a Windows Service</span></span>

<span data-ttu-id="56f0a-109">Následující minimální změny je potřeba nastavit stávající projekt ASP.NET Core pro spouštěn jako služba:</span><span class="sxs-lookup"><span data-stu-id="56f0a-109">The following minimum changes are required to set up an existing ASP.NET Core project to run as a service:</span></span>

1. <span data-ttu-id="56f0a-110">V souboru projektu:</span><span class="sxs-lookup"><span data-stu-id="56f0a-110">In the project file:</span></span>

   * <span data-ttu-id="56f0a-111">Ověřte existenci Windows [identifikátor modulu Runtime (RID)](/dotnet/core/rid-catalog) nebo ho přidat do `<PropertyGroup>` , který obsahuje Cílová architektura:</span><span class="sxs-lookup"><span data-stu-id="56f0a-111">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add it to the `<PropertyGroup>` that contains the target framework:</span></span>

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

      <span data-ttu-id="56f0a-112">Chcete-li publikovat pro více identifikátorů RID:</span><span class="sxs-lookup"><span data-stu-id="56f0a-112">To publish for multiple RIDs:</span></span>

      * <span data-ttu-id="56f0a-113">Zadejte identifikátory RID v seznam oddělený středníkem.</span><span class="sxs-lookup"><span data-stu-id="56f0a-113">Provide the RIDs in a semicolon-delimited list.</span></span>
      * <span data-ttu-id="56f0a-114">Použijte název vlastnosti `<RuntimeIdentifiers>` (množné číslo).</span><span class="sxs-lookup"><span data-stu-id="56f0a-114">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

      <span data-ttu-id="56f0a-115">Další informace najdete v tématu [katalog identifikátorů RID .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="56f0a-115">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="56f0a-116">Přidat odkaz na balíček pro [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="56f0a-116">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

1. <span data-ttu-id="56f0a-117">Proveďte následující změny v `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="56f0a-117">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="56f0a-118">Volání [hostitele. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) místo `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="56f0a-118">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="56f0a-119">Volání [UseContentRoot](xref:fundamentals/host/web-host#content-root) a cesta k aplikaci pro publikování umístění, namísto použití `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="56f0a-119">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="56f0a-120">Publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="56f0a-120">Publish the app.</span></span> <span data-ttu-id="56f0a-121">Použití [dotnet publikovat](/dotnet/articles/core/tools/dotnet-publish) nebo [profil publikování pro Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="56f0a-121">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span> <span data-ttu-id="56f0a-122">Při používání sady Visual Studio, vyberte **FolderProfile**.</span><span class="sxs-lookup"><span data-stu-id="56f0a-122">When using a Visual Studio, select the **FolderProfile**.</span></span>

   <span data-ttu-id="56f0a-123">Chcete-li publikovat ukázkovou aplikaci pomocí nástrojů rozhraní příkazového řádku (CLI), spusťte [dotnet publikovat](/dotnet/core/tools/dotnet-publish) příkazu na příkazovém řádku ve složce projektu.</span><span class="sxs-lookup"><span data-stu-id="56f0a-123">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder.</span></span> <span data-ttu-id="56f0a-124">V musí být zadán identifikátor RID `<RuntimeIdenfifier>` (nebo `<RuntimeIdentifiers>`) vlastnost souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="56f0a-124">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="56f0a-125">V následujícím příkladu, publikování aplikace v rámci konfigurace verze pro `win7-x64` modulu runtime:</span><span class="sxs-lookup"><span data-stu-id="56f0a-125">In the following example, the app is published in Release configuration for the `win7-x64` runtime:</span></span>

   ```console
   dotnet publish --configuration Release --runtime win7-x64
   ```

1. <span data-ttu-id="56f0a-126">Použití [sc.exe](https://technet.microsoft.com/library/bb490995) nástroj příkazového řádku vytvořte službu.</span><span class="sxs-lookup"><span data-stu-id="56f0a-126">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="56f0a-127">`binPath` Hodnota je cesta ke spustitelnému souboru aplikace, která zahrnuje název spustitelného souboru.</span><span class="sxs-lookup"><span data-stu-id="56f0a-127">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="56f0a-128">**Mezera mezi znaménko rovná se a znak uvozovek na začátek cesty je povinný.**</span><span class="sxs-lookup"><span data-stu-id="56f0a-128">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="56f0a-129">Služby se publikují do složky projektu, použijte cestu k *publikovat* složku pro vytvoření služby.</span><span class="sxs-lookup"><span data-stu-id="56f0a-129">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="56f0a-130">V následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="56f0a-130">In the following example:</span></span>

   * <span data-ttu-id="56f0a-131">Projekt se nachází v *c:\\my_services\\AspNetCoreService* složky.</span><span class="sxs-lookup"><span data-stu-id="56f0a-131">The project resides in the *c:\\my_services\\AspNetCoreService* folder.</span></span>
   * <span data-ttu-id="56f0a-132">Projekt, se publikují v `Release` konfigurace.</span><span class="sxs-lookup"><span data-stu-id="56f0a-132">The project is published in `Release` configuration.</span></span>
   * <span data-ttu-id="56f0a-133">Moniker cílového rozhraní (TFM) je `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="56f0a-133">The Target Framework Moniker (TFM) is `netcoreapp2.1`.</span></span>
   * <span data-ttu-id="56f0a-134">Identifikátor modulu Runtime (RID) je `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="56f0a-134">The Runtime Identifer (RID) is `win7-x64`.</span></span>
   * <span data-ttu-id="56f0a-135">Je název spustitelné aplikace *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="56f0a-135">The app executable is named *AspNetCoreService.exe*.</span></span>
   * <span data-ttu-id="56f0a-136">Služba má název **Moje_služba**.</span><span class="sxs-lookup"><span data-stu-id="56f0a-136">The service is named **MyService**.</span></span>

   <span data-ttu-id="56f0a-137">Příklad:</span><span class="sxs-lookup"><span data-stu-id="56f0a-137">Example:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="56f0a-138">Ujistěte se, že je k dispozici mezi prostor `binPath=` argument a její hodnotu.</span><span class="sxs-lookup"><span data-stu-id="56f0a-138">Make sure the space is present between the `binPath=` argument and its value.</span></span>

   <span data-ttu-id="56f0a-139">Publikovat a spustit službu z jiné složky:</span><span class="sxs-lookup"><span data-stu-id="56f0a-139">To publish and start the service from a different folder:</span></span>

      * <span data-ttu-id="56f0a-140">Použití [– výstupní &lt;OUTPUT_DIRECTORY&gt; ](/dotnet/core/tools/dotnet-publish#options) možnost `dotnet publish` příkazu.</span><span class="sxs-lookup"><span data-stu-id="56f0a-140">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span> <span data-ttu-id="56f0a-141">Pokud používáte Visual Studio, nakonfigurujte **cílové umístění** v **FolderProfile** publikovat stránku vlastností před výběrem **publikovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="56f0a-141">If using Visual Studio, configure the **Target Location** in the **FolderProfile** publish property page before selecting the **Publish** button.</span></span>
      * <span data-ttu-id="56f0a-142">Vytvoření služby s `sc.exe` příkazu cesta k výstupní složce.</span><span class="sxs-lookup"><span data-stu-id="56f0a-142">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="56f0a-143">Zahrnout název spustitelného souboru služby na cestě k dispozici na `binPath`.</span><span class="sxs-lookup"><span data-stu-id="56f0a-143">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="56f0a-144">Spusťte službu pomocí `sc start <SERVICE_NAME>` příkazu.</span><span class="sxs-lookup"><span data-stu-id="56f0a-144">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="56f0a-145">Spustit službu ukázkové aplikace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="56f0a-145">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="56f0a-146">Příkaz trvá několik sekund se spustit službu.</span><span class="sxs-lookup"><span data-stu-id="56f0a-146">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="56f0a-147">Chcete-li zkontrolovat stav služby, použijte `sc query <SERVICE_NAME>` příkazu.</span><span class="sxs-lookup"><span data-stu-id="56f0a-147">To check the status of the service, use the `sc query <SERVICE_NAME>` command.</span></span> <span data-ttu-id="56f0a-148">Stav je uveden jako jeden z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="56f0a-148">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="56f0a-149">Použijte následující příkaz a zkontrolujte stav služby app service vzorku:</span><span class="sxs-lookup"><span data-stu-id="56f0a-149">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="56f0a-150">Pokud je služba v `RUNNING` stavu a pokud je služba webové aplikace, procházet aplikace, její cesta (ve výchozím nastavení, `http://localhost:5000`, který přesměruje `https://localhost:5001` při použití [HTTPS přesměrování Middleware](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="56f0a-150">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="56f0a-151">Aplikační služba ukázkového procházet aplikace na adrese `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="56f0a-151">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="56f0a-152">Zastavit službu s `sc stop <SERVICE_NAME>` příkazu.</span><span class="sxs-lookup"><span data-stu-id="56f0a-152">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="56f0a-153">Následující příkaz zastaví aplikační služba ukázkového:</span><span class="sxs-lookup"><span data-stu-id="56f0a-153">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="56f0a-154">Po krátké prodlevě zastavit službu, odinstalujte službu s `sc delete <SERVICE_NAME>` příkazu.</span><span class="sxs-lookup"><span data-stu-id="56f0a-154">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="56f0a-155">Postup kontroly stavu aplikační služba ukázkového:</span><span class="sxs-lookup"><span data-stu-id="56f0a-155">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="56f0a-156">Pokud je aplikační služba ukázkového v `STOPPED` stavu, použijte následující příkaz pro odinstalaci aplikační služba ukázkového:</span><span class="sxs-lookup"><span data-stu-id="56f0a-156">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a><span data-ttu-id="56f0a-157">Spuštění aplikace mimo službu</span><span class="sxs-lookup"><span data-stu-id="56f0a-157">Run the app outside of a service</span></span>

<span data-ttu-id="56f0a-158">Usnadňuje testování a ladění, když se provozují mimo službu, takže je obvyklý přidáte kód, který volá `RunAsService` pouze za určitých podmínek.</span><span class="sxs-lookup"><span data-stu-id="56f0a-158">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="56f0a-159">Například aplikace může běžet jako aplikace konzoly v jazyce `--console` argument příkazového řádku nebo pokud je připojen ladicí program:</span><span class="sxs-lookup"><span data-stu-id="56f0a-159">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="56f0a-160">Vzhledem k tomu, že konfigurace ASP.NET Core vyžaduje páry název hodnota pro argumenty příkazového řádku, `--console` přepínač byl předtím, než jsou předány argumenty [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="56f0a-160">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="56f0a-161">`isService` nebude zadán, využije z `Main` do `CreateWebHostBuilder` protože podpis `CreateWebHostBuilder` musí být `CreateWebHostBuilder(string[])` mohl [testování integrace](xref:test/integration-tests) fungovala správně.</span><span class="sxs-lookup"><span data-stu-id="56f0a-161">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="56f0a-162">Zpracování, spouštění a zastavování události</span><span class="sxs-lookup"><span data-stu-id="56f0a-162">Handle stopping and starting events</span></span>

<span data-ttu-id="56f0a-163">Pro zpracování [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), a [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) události, proveďte následující další změny:</span><span class="sxs-lookup"><span data-stu-id="56f0a-163">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="56f0a-164">Vytvořte třídu, která je odvozena z [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="56f0a-164">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="56f0a-165">Vytvořit metodu rozšíření pro [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) , který předá vlastní `WebHostService` k [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="56f0a-165">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="56f0a-166">V `Program.Main`, zavolat novou metodu rozšíření `RunAsCustomService`, namísto [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="56f0a-166">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="56f0a-167">`isService` nebude zadán, využije z `Main` do `CreateWebHostBuilder` protože podpis `CreateWebHostBuilder` musí být `CreateWebHostBuilder(string[])` mohl [testování integrace](xref:test/integration-tests) fungovala správně.</span><span class="sxs-lookup"><span data-stu-id="56f0a-167">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="56f0a-168">Pokud vlastní `WebHostService` kód vyžaduje službu z injektáž závislostí (jako je například protokolování), získat ze [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) vlastnost:</span><span class="sxs-lookup"><span data-stu-id="56f0a-168">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="56f0a-169">Proxy server a scénáře pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="56f0a-169">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="56f0a-170">Služby, které interakci s žádostí z Internetu nebo podnikové síti a jsou za proxy nebo nástroj pro vyrovnávání zatížení může vyžadovat dodatečnou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="56f0a-170">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="56f0a-171">Další informace naleznete v tématu <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="56f0a-171">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="56f0a-172">Konfigurace HTTPS</span><span class="sxs-lookup"><span data-stu-id="56f0a-172">Configure HTTPS</span></span>

<span data-ttu-id="56f0a-173">Jak nakonfigurovat službu s koncovým bodem zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="56f0a-173">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="56f0a-174">Vytvořte certifikát X.509, který pro hostování systému získání certifikátů vaší platformě a mechanismy nasazení.</span><span class="sxs-lookup"><span data-stu-id="56f0a-174">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>
1. <span data-ttu-id="56f0a-175">Zadejte [konfigurace koncového bodu HTTPS serveru Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) mohl certifikát používat.</span><span class="sxs-lookup"><span data-stu-id="56f0a-175">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="56f0a-176">Použití certifikátu vývoj pro ASP.NET Core HTTPS k zabezpečení koncového bodu služby se nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="56f0a-176">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="56f0a-177">Aktuální adresář a obsahu root</span><span class="sxs-lookup"><span data-stu-id="56f0a-177">Current directory and content root</span></span>

<span data-ttu-id="56f0a-178">Aktuální pracovní adresář vrátit voláním `Directory.GetCurrentDirectory()` pro službu Windows je *C:\\WINDOWS\\system32* složky.</span><span class="sxs-lookup"><span data-stu-id="56f0a-178">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="56f0a-179">*System32* složka není vhodné umístění pro ukládání souborů služby (například soubory nastavení).</span><span class="sxs-lookup"><span data-stu-id="56f0a-179">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="56f0a-180">Použijte jednu z následujících dvou přístupů k zajištění údržby a přístup k prostředky a nastavení souborů pomocí služby [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) při použití [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="56f0a-180">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="56f0a-181">Pomocí obsahu kořenovou cestu.</span><span class="sxs-lookup"><span data-stu-id="56f0a-181">Use the content root path.</span></span> <span data-ttu-id="56f0a-182">`IHostingEnvironment.ContentRootPath` Se stejnou cestu k dispozici na `binPath` argument při vytvoření služby.</span><span class="sxs-lookup"><span data-stu-id="56f0a-182">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="56f0a-183">Namísto použití `Directory.GetCurrentDirectory()` k vytvoření cesty k souborům nastavení, použijte obsah kořenové cestě a udržovat soubory v kořenové obsahu aplikace.</span><span class="sxs-lookup"><span data-stu-id="56f0a-183">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="56f0a-184">Store soubory na vhodné místo na disku.</span><span class="sxs-lookup"><span data-stu-id="56f0a-184">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="56f0a-185">Zadejte absolutní cestu s `SetBasePath` do složky obsahující soubory.</span><span class="sxs-lookup"><span data-stu-id="56f0a-185">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56f0a-186">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="56f0a-186">Additional resources</span></span>

* <span data-ttu-id="56f0a-187">[Konfigurace koncového bodu kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (včetně konfigurace protokolu HTTPS a podporu SNI)</span><span class="sxs-lookup"><span data-stu-id="56f0a-187">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
