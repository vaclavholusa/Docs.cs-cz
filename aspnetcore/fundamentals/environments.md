---
title: "Práce s několika prostředí v ASP.NET Core"
author: rick-anderson
description: "Zjistěte, jak ASP.NET Core poskytuje podporu pro řízení chování aplikace ve více prostředích."
keywords: "ASP.NET Core, nastavení prostředí, ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 784d176145c3e4e44ddc0ea06b6702f70cd4b08c
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="b8ced-104">Práce s několika prostředí</span><span class="sxs-lookup"><span data-stu-id="b8ced-104">Working with multiple environments</span></span>

<span data-ttu-id="b8ced-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b8ced-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b8ced-106">Základní technologie ASP.NET poskytuje podporu pro nastavení chování aplikace za běhu pomocí proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="b8ced-106">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="b8ced-107">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b8ced-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="b8ced-108">Prostředí</span><span class="sxs-lookup"><span data-stu-id="b8ced-108">Environments</span></span>

<span data-ttu-id="b8ced-109">Přečte proměnné prostředí ASP.NET Core `ASPNETCORE_ENVIRONMENT` při spuštění aplikace a úložišť, která hodnotu [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="b8ced-109">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="b8ced-110">`ASPNETCORE_ENVIRONMENT`můžete nastavit na jakoukoli hodnotu, ale [tří hodnot](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) podporuje rozhraní: [vývoj](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [pracovní](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), a [produkční](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="b8ced-110">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="b8ced-111">Pokud `ASPNETCORE_ENVIRONMENT` není nastaven, bude použita výchozí `Production`.</span><span class="sxs-lookup"><span data-stu-id="b8ced-111">If `ASPNETCORE_ENVIRONMENT` is not set, it will default to `Production`.</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="b8ced-112">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="b8ced-112">The preceding code:</span></span>

* <span data-ttu-id="b8ced-113">Volání [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) a [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) při `ASPNETCORE_ENVIRONMENT` je nastaven na `Development`.</span><span class="sxs-lookup"><span data-stu-id="b8ced-113">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="b8ced-114">Volání [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) při hodnotě `ASPNETCORE_ENVIRONMENT` je nastavit jednu z následujících:</span><span class="sxs-lookup"><span data-stu-id="b8ced-114">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="b8ced-115">[Pomocná značku prostředí ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) používá hodnotu `IHostingEnvironment.EnvironmentName` pro zahrnutí nebo vyloučení značek v elementu:</span><span class="sxs-lookup"><span data-stu-id="b8ced-115">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="b8ced-116">Poznámka: V systémech Windows a systému macOS proměnné prostředí a hodnoty nejsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="b8ced-116">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="b8ced-117">Proměnné prostředí Linux a jejich hodnoty jsou **velká a malá písmena** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="b8ced-117">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="b8ced-118">Vývoj</span><span class="sxs-lookup"><span data-stu-id="b8ced-118">Development</span></span>

<span data-ttu-id="b8ced-119">Vývojové prostředí můžete povolit funkce, které by neměly být vystaveny v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b8ced-119">The development environment can enable features that should not be exposed in production.</span></span> <span data-ttu-id="b8ced-120">Například šablony ASP.NET Core povolit [vývojáře výjimka stránky](xref:fundamentals/error-handling#the-developer-exception-page) ve vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="b8ced-120">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="b8ced-121">Prostředí pro vývoj na místním počítači může být nastavena v *Properties\launchSettings.json* souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="b8ced-121">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="b8ced-122">Hodnoty prostředí nastavené *launchSettings.json* přepsat hodnoty nastavené v prostředí systému.</span><span class="sxs-lookup"><span data-stu-id="b8ced-122">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="b8ced-123">Následující kód XML ukazuje tři profily *launchSettings.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="b8ced-123">The following XML shows three profiles from a *launchSettings.json* file:</span></span>

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="b8ced-124">Když je aplikace spuštěna s `dotnet run`, první profil s `"commandName": "Project"` se použije.</span><span class="sxs-lookup"><span data-stu-id="b8ced-124">When the application is launched with `dotnet run`, the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="b8ced-125">Hodnota `commandName` určuje webový server ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="b8ced-125">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="b8ced-126">`commandName`může být jedna z:</span><span class="sxs-lookup"><span data-stu-id="b8ced-126">`commandName` can be one of :</span></span>

* <span data-ttu-id="b8ced-127">Služby IIS Express</span><span class="sxs-lookup"><span data-stu-id="b8ced-127">IIS Express</span></span>
* <span data-ttu-id="b8ced-128">IIS</span><span class="sxs-lookup"><span data-stu-id="b8ced-128">IIS</span></span>
* <span data-ttu-id="b8ced-129">Projekt (který se spouští Kestrel)</span><span class="sxs-lookup"><span data-stu-id="b8ced-129">Project (which launches Kestrel)</span></span>

<span data-ttu-id="b8ced-130">Když se aplikace spustí s `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="b8ced-130">When an app is launched with `dotnet run`:</span></span>

* <span data-ttu-id="b8ced-131">*launchSettings.json* je pro čtení. Pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b8ced-131">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="b8ced-132">`environmentVariables`nastavení v *launchSettings.json* přepsat proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="b8ced-132">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="b8ced-133">Hostování prostředí bude zobrazeno.</span><span class="sxs-lookup"><span data-stu-id="b8ced-133">The hosting environment is displayed.</span></span>


<span data-ttu-id="b8ced-134">Následující výstup zobrazuje aplikace spuštěna s `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="b8ced-134">The following output shows an app started with `dotnet run`:</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="b8ced-135">Visual Studio **ladění** karta poskytuje grafickým uživatelským rozhraním upravit *launchSettings.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="b8ced-135">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proměnné prostředí nastavení vlastností projektu](environments/_static/project-properties-debug.png)

<span data-ttu-id="b8ced-137">Změny provedené v projektu profily pravděpodobně projeví až po restartu webového serveru.</span><span class="sxs-lookup"><span data-stu-id="b8ced-137">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="b8ced-138">Kestrel zjistí změny provedené v jeho prostředí musí být restartován.</span><span class="sxs-lookup"><span data-stu-id="b8ced-138">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="b8ced-139">*launchSettings.json* neměli ukládat tajné klíče.</span><span class="sxs-lookup"><span data-stu-id="b8ced-139">*launchSettings.json* should not store secrets.</span></span> <span data-ttu-id="b8ced-140">[Nástroj tajný klíč správce](xref:security/app-secrets) slouží k uložení tajné klíče pro místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="b8ced-140">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="b8ced-141">Produkční</span><span class="sxs-lookup"><span data-stu-id="b8ced-141">Production</span></span>

<span data-ttu-id="b8ced-142">Maximalizovat zabezpečení, výkonu a odolnosti aplikace musí být nakonfigurovaný v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b8ced-142">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="b8ced-143">Některé běžné nastavení, která může mít provozním prostředí které by se liší od vývoj patří:</span><span class="sxs-lookup"><span data-stu-id="b8ced-143">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="b8ced-144">Ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="b8ced-144">Caching.</span></span>
* <span data-ttu-id="b8ced-145">Prostředky na straně klienta jsou seskupeny, minifikovaný a potenciálně zpracovat z název CDN.</span><span class="sxs-lookup"><span data-stu-id="b8ced-145">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="b8ced-146">Diagnostické chybové stránky zakázána.</span><span class="sxs-lookup"><span data-stu-id="b8ced-146">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="b8ced-147">Povolit popisný chybové stránky.</span><span class="sxs-lookup"><span data-stu-id="b8ced-147">Friendly error pages enabled.</span></span>
* <span data-ttu-id="b8ced-148">Produkční protokolování a monitorování povoleno.</span><span class="sxs-lookup"><span data-stu-id="b8ced-148">Production logging and monitoring enabled.</span></span> <span data-ttu-id="b8ced-149">Například [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span><span class="sxs-lookup"><span data-stu-id="b8ced-149">For example, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="b8ced-150">Nastavení prostředí</span><span class="sxs-lookup"><span data-stu-id="b8ced-150">Setting the environment</span></span>

<span data-ttu-id="b8ced-151">Často je vhodné nastavit konkrétní prostředí pro testování.</span><span class="sxs-lookup"><span data-stu-id="b8ced-151">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="b8ced-152">Pokud není nastavena v prostředí, bude výchozí `Production` který zakáže většinu funkcí ladění.</span><span class="sxs-lookup"><span data-stu-id="b8ced-152">If the environment is not set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="b8ced-153">Metoda pro nastavení prostředí závisí na operačním systému.</span><span class="sxs-lookup"><span data-stu-id="b8ced-153">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="b8ced-154">Azure</span><span class="sxs-lookup"><span data-stu-id="b8ced-154">Azure</span></span>

<span data-ttu-id="b8ced-155">Pro službu Azure app service:</span><span class="sxs-lookup"><span data-stu-id="b8ced-155">For Azure app service:</span></span>

* <span data-ttu-id="b8ced-156">Vyberte **nastavení aplikace** okno.</span><span class="sxs-lookup"><span data-stu-id="b8ced-156">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="b8ced-157">Přidejte klíč a hodnotu **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b8ced-157">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="b8ced-158">Windows</span><span class="sxs-lookup"><span data-stu-id="b8ced-158">Windows</span></span>
<span data-ttu-id="b8ced-159">Chcete-li nastavit `ASPNETCORE_ENVIRONMENT` pro aktuální relaci, pokud je aplikace spuštěná pomocí `dotnet run`, se používají následující příkazy</span><span class="sxs-lookup"><span data-stu-id="b8ced-159">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="b8ced-160">**Příkazový řádek**</span><span class="sxs-lookup"><span data-stu-id="b8ced-160">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="b8ced-161">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="b8ced-161">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="b8ced-162">Tyto příkazy se projeví pouze pro aktuální okno.</span><span class="sxs-lookup"><span data-stu-id="b8ced-162">These commands take effect only for the current window.</span></span> <span data-ttu-id="b8ced-163">Při zavření okna nastavení ASPNETCORE_ENVIRONMENT obnoví výchozí nastavení nebo počítač hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b8ced-163">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="b8ced-164">Chcete-li nastavit hodnotu globálně při otevření Windows **ovládací panely** > **systému** > **Upřesnit nastavení systému** a přidat nebo upravit `ASPNETCORE_ENVIRONMENT` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b8ced-164">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Rozšířené vlastnosti systému](environments/_static/systemsetting_environment.png)

![Proměnné prostředí základní ASPNET](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="b8ced-167">**web.config**</span><span class="sxs-lookup"><span data-stu-id="b8ced-167">**web.config**</span></span>

<span data-ttu-id="b8ced-168">Najdete v článku *nastavení proměnných prostředí* části [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tématu.</span><span class="sxs-lookup"><span data-stu-id="b8ced-168">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="b8ced-169">**Fondy aplikací služby IIS**</span><span class="sxs-lookup"><span data-stu-id="b8ced-169">**Per IIS Application Pool**</span></span>

<span data-ttu-id="b8ced-170">Nastavení proměnných prostředí pro jednotlivé aplikace běžící v izolované fondy aplikací (podporuje se ve službě IIS 10.0 +), najdete v článku *příkazu AppCmd.exe* části [proměnné prostředí \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tématu.</span><span class="sxs-lookup"><span data-stu-id="b8ced-170">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="b8ced-171">macOS</span><span class="sxs-lookup"><span data-stu-id="b8ced-171">macOS</span></span>
<span data-ttu-id="b8ced-172">Nastavení aktuální prostředí pro systému macOS lze provést v řádku při spuštění aplikace;</span><span class="sxs-lookup"><span data-stu-id="b8ced-172">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="b8ced-173">nebo pomocí `export` pro ni nastavit před spuštěním aplikace.</span><span class="sxs-lookup"><span data-stu-id="b8ced-173">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="b8ced-174">Proměnné prostředí úrovni počítače jsou nastaveny v *.bashrc* nebo *.bash_profile* souboru.</span><span class="sxs-lookup"><span data-stu-id="b8ced-174">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="b8ced-175">Upravte soubor pomocí libovolného textového editoru a přidejte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="b8ced-175">Edit the file using any text editor and add the following statment.</span></span>

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="b8ced-176">Linux</span><span class="sxs-lookup"><span data-stu-id="b8ced-176">Linux</span></span>
<span data-ttu-id="b8ced-177">Pro distribucích systému Linux, použijte `export` příkaz na příkazovém řádku pro relace na základě nastavení proměnné a *bash_profile* soubor pro nastavení úrovně prostředí počítače.</span><span class="sxs-lookup"><span data-stu-id="b8ced-177">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="b8ced-178">Konfigurace prostředí</span><span class="sxs-lookup"><span data-stu-id="b8ced-178">Configuration by environment</span></span>

<span data-ttu-id="b8ced-179">V tématu [konfigurace prostředí](xref:fundamentals/configuration/index#configuration-by-environment) Další informace.</span><span class="sxs-lookup"><span data-stu-id="b8ced-179">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="b8ced-180">Prostředí na základě spuštění třídy a metody</span><span class="sxs-lookup"><span data-stu-id="b8ced-180">Environment based Startup class and methods</span></span>

<span data-ttu-id="b8ced-181">Při spuštění aplikace ASP.NET Core [třída při spuštění](xref:fundamentals/startup) bootstraps aplikace.</span><span class="sxs-lookup"><span data-stu-id="b8ced-181">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="b8ced-182">Pokud se třída `Startup{EnvironmentName}` existuje, že třídu bude volat třída pro tento `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="b8ced-182">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="b8ced-183">Poznámka: Volání [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) přepsání konfigurační oddíly.</span><span class="sxs-lookup"><span data-stu-id="b8ced-183">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="b8ced-184">[Konfigurace](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) a [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) podporu prostředí konkrétních verzích formuláře `Configure{EnvironmentName}` a `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="b8ced-184">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="b8ced-185">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="b8ced-185">Additional Resources</span></span>

* [<span data-ttu-id="b8ced-186">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b8ced-186">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="b8ced-187">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="b8ced-187">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="b8ced-188">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="b8ced-188">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
