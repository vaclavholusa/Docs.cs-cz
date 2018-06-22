---
title: Použití prostředí s více v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak ASP.NET Core poskytuje podporu pro řízení chování aplikace ve více prostředích.
ms.author: riande
ms.date: 12/25/2017
uid: fundamentals/environments
ms.openlocfilehash: 5a4caeeba045cb93dec9c73c931dae8a352bede9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276929"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="3b0a1-103">Použití prostředí s více v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3b0a1-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="3b0a1-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3b0a1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3b0a1-105">Základní technologie ASP.NET poskytuje podporu pro nastavení chování aplikace za běhu pomocí proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="3b0a1-106">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3b0a1-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="3b0a1-107">Prostředí</span><span class="sxs-lookup"><span data-stu-id="3b0a1-107">Environments</span></span>

<span data-ttu-id="3b0a1-108">Přečte proměnné prostředí ASP.NET Core `ASPNETCORE_ENVIRONMENT` při spuštění aplikace a úložišť, která hodnotu [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="3b0a1-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="3b0a1-109">`ASPNETCORE_ENVIRONMENT` můžete nastavit na jakoukoli hodnotu, ale [tří hodnot](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) podporuje rozhraní: [vývoj](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [pracovní](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), a [produkční](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="3b0a1-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="3b0a1-110">Pokud `ASPNETCORE_ENVIRONMENT` není nastaven, bude použita výchozí `Production`.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="3b0a1-111">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="3b0a1-111">The preceding code:</span></span>

* <span data-ttu-id="3b0a1-112">Volání [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) a [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) při `ASPNETCORE_ENVIRONMENT` je nastaven na `Development`.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="3b0a1-113">Volání [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) při hodnotě `ASPNETCORE_ENVIRONMENT` je nastavit jednu z následujících:</span><span class="sxs-lookup"><span data-stu-id="3b0a1-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="3b0a1-114">[Pomocná značku prostředí ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) používá hodnotu `IHostingEnvironment.EnvironmentName` pro zahrnutí nebo vyloučení značek v elementu:</span><span class="sxs-lookup"><span data-stu-id="3b0a1-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="3b0a1-115">Poznámka: V systémech Windows a systému macOS proměnné prostředí a hodnoty nejsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="3b0a1-116">Proměnné prostředí Linux a jejich hodnoty jsou **velká a malá písmena** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="3b0a1-117">Vývoj</span><span class="sxs-lookup"><span data-stu-id="3b0a1-117">Development</span></span>

<span data-ttu-id="3b0a1-118">Vývojové prostředí můžete povolit funkce, které by neměly být vystaveny v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="3b0a1-119">Například šablony ASP.NET Core povolit [vývojáře výjimka stránky](xref:fundamentals/error-handling#the-developer-exception-page) ve vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="3b0a1-120">Prostředí pro vývoj na místním počítači může být nastavena v *Properties\launchSettings.json* souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="3b0a1-121">Hodnoty prostředí nastavené *launchSettings.json* přepsat hodnoty nastavené v prostředí systému.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="3b0a1-122">Zobrazuje tři profily z následujícím kódu JSON *launchSettings.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="3b0a1-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> <span data-ttu-id="3b0a1-123">`applicationUrl` Vlastnost *launchSettings.json* můžete zadat seznam adres URL serveru.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="3b0a1-124">Použijte středník mezi adresy URL v seznamu:</span><span class="sxs-lookup"><span data-stu-id="3b0a1-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "WebApplication1": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```
::: moniker-end

<span data-ttu-id="3b0a1-125">Když je aplikace spuštěna s [dotnet spustit](/dotnet/core/tools/dotnet-run), první profil s `"commandName": "Project"` se použije.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-125">When the application is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="3b0a1-126">Hodnota `commandName` určuje webový server ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="3b0a1-127">`commandName` může být jedna z:</span><span class="sxs-lookup"><span data-stu-id="3b0a1-127">`commandName` can be one of :</span></span>

* <span data-ttu-id="3b0a1-128">Služby IIS Express</span><span class="sxs-lookup"><span data-stu-id="3b0a1-128">IIS Express</span></span>
* <span data-ttu-id="3b0a1-129">IIS</span><span class="sxs-lookup"><span data-stu-id="3b0a1-129">IIS</span></span>
* <span data-ttu-id="3b0a1-130">Projekt (který se spouští Kestrel)</span><span class="sxs-lookup"><span data-stu-id="3b0a1-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="3b0a1-131">Když se aplikace spustí s [dotnet spustit](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="3b0a1-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="3b0a1-132">*launchSettings.json* je pro čtení. Pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="3b0a1-133">`environmentVariables` nastavení v *launchSettings.json* přepsat proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="3b0a1-134">Hostování prostředí bude zobrazeno.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-134">The hosting environment is displayed.</span></span>

<span data-ttu-id="3b0a1-135">Následující výstup zobrazuje aplikace spuštěna s [dotnet spustit](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="3b0a1-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="3b0a1-136">Visual Studio **ladění** karta poskytuje grafickým uživatelským rozhraním upravit *launchSettings.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="3b0a1-136">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proměnné prostředí nastavení vlastností projektu](environments/_static/project-properties-debug.png)

<span data-ttu-id="3b0a1-138">Změny provedené v projektu profily pravděpodobně projeví až po restartu webového serveru.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="3b0a1-139">Kestrel zjistí změny provedené v jeho prostředí musí být restartován.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-139">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="3b0a1-140">*launchSettings.json* neměli ukládat tajné klíče.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="3b0a1-141">[Nástroj tajný klíč správce](xref:security/app-secrets) slouží k uložení tajné klíče pro místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="3b0a1-142">Při použití [Visual Studio Code](https://code.visualstudio.com/), proměnné prostředí může být nastavena v *.vscode/launch.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-142">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="3b0a1-143">Následující příklad nastaví prostředí `Development`:</span><span class="sxs-lookup"><span data-stu-id="3b0a1-143">The following example sets the environment to `Development`:</span></span>

```json
{
   "version": "0.2.0",
   "configurations": [
        {
            "name": ".NET Core Launch (web)",

            ... additional VS Code configuration settings ...

            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

<span data-ttu-id="3b0a1-144">A *.vscode/launch.json* soubor v projektu není přečíst při spuštění aplikace s `dotnet run` stejným způsobem jako *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-144">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="3b0a1-145">Při spuštění aplikace v vývoj, který nemá *launchSettings.json* souboru nastavená buď v prostředí s proměnné prostředí nebo argument příkazového řádku k `dotnet run` příkaz.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-145">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="3b0a1-146">Produkční</span><span class="sxs-lookup"><span data-stu-id="3b0a1-146">Production</span></span>

<span data-ttu-id="3b0a1-147">Maximalizovat zabezpečení, výkonu a odolnosti aplikace musí být nakonfigurovaný v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-147">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="3b0a1-148">Některé běžné nastavení, které se liší od vývoj patří:</span><span class="sxs-lookup"><span data-stu-id="3b0a1-148">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="3b0a1-149">Ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-149">Caching.</span></span>
* <span data-ttu-id="3b0a1-150">Prostředky na straně klienta jsou seskupeny, minifikovaný a potenciálně zpracovat z název CDN.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-150">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="3b0a1-151">Diagnostické chybové stránky zakázána.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-151">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="3b0a1-152">Povolit popisný chybové stránky.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-152">Friendly error pages enabled.</span></span>
* <span data-ttu-id="3b0a1-153">Produkční protokolování a monitorování povoleno.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-153">Production logging and monitoring enabled.</span></span> <span data-ttu-id="3b0a1-154">Například [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="3b0a1-154">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="3b0a1-155">Nastavení prostředí</span><span class="sxs-lookup"><span data-stu-id="3b0a1-155">Setting the environment</span></span>

<span data-ttu-id="3b0a1-156">Často je vhodné nastavit konkrétní prostředí pro testování.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-156">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="3b0a1-157">Pokud není nastavena v prostředí, bude výchozí `Production` který zakáže většinu funkcí ladění.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-157">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="3b0a1-158">Metoda pro nastavení prostředí závisí na operačním systému.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-158">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="3b0a1-159">Azure</span><span class="sxs-lookup"><span data-stu-id="3b0a1-159">Azure</span></span>

<span data-ttu-id="3b0a1-160">Pro službu Azure app service:</span><span class="sxs-lookup"><span data-stu-id="3b0a1-160">For Azure app service:</span></span>

* <span data-ttu-id="3b0a1-161">Vyberte **nastavení aplikace** okno.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-161">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="3b0a1-162">Přidejte klíč a hodnotu **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-162">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="3b0a1-163">Windows</span><span class="sxs-lookup"><span data-stu-id="3b0a1-163">Windows</span></span>
<span data-ttu-id="3b0a1-164">Chcete-li nastavit `ASPNETCORE_ENVIRONMENT` pro aktuální relaci, pokud je aplikace spuštěná pomocí [dotnet spustit](/dotnet/core/tools/dotnet-run), se používají následující příkazy</span><span class="sxs-lookup"><span data-stu-id="3b0a1-164">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used</span></span>

<span data-ttu-id="3b0a1-165">**Příkazový řádek**</span><span class="sxs-lookup"><span data-stu-id="3b0a1-165">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="3b0a1-166">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="3b0a1-166">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="3b0a1-167">Tyto příkazy se projeví pouze pro aktuální okno.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-167">These commands take effect only for the current window.</span></span> <span data-ttu-id="3b0a1-168">Při zavření okna nastavení ASPNETCORE_ENVIRONMENT obnoví výchozí nastavení nebo počítač hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-168">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="3b0a1-169">Chcete-li nastavit hodnotu globálně při otevření Windows **ovládací panely** > **systému** > **Upřesnit nastavení systému** a přidat nebo upravit `ASPNETCORE_ENVIRONMENT` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-169">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Rozšířené vlastnosti systému](environments/_static/systemsetting_environment.png)

![Proměnné prostředí základní ASPNET](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="3b0a1-172">**web.config**</span><span class="sxs-lookup"><span data-stu-id="3b0a1-172">**web.config**</span></span>

<span data-ttu-id="3b0a1-173">Najdete v článku *nastavení proměnných prostředí* části [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tématu.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-173">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="3b0a1-174">**Fondy aplikací služby IIS**</span><span class="sxs-lookup"><span data-stu-id="3b0a1-174">**Per IIS Application Pool**</span></span>

<span data-ttu-id="3b0a1-175">Nastavení proměnných prostředí pro jednotlivé aplikace běžící v izolované fondy aplikací (podporuje se ve službě IIS 10.0 +), najdete v článku *příkazu AppCmd.exe* části [proměnné prostředí \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tématu.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-175">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="3b0a1-176">macOS</span><span class="sxs-lookup"><span data-stu-id="3b0a1-176">macOS</span></span>
<span data-ttu-id="3b0a1-177">Nastavení aktuální prostředí pro systému macOS lze provést v řádku při spuštění aplikace;</span><span class="sxs-lookup"><span data-stu-id="3b0a1-177">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="3b0a1-178">nebo pomocí `export` pro ni nastavit před spuštěním aplikace.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-178">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="3b0a1-179">Proměnné prostředí úrovni počítače jsou nastaveny v *.bashrc* nebo *.bash_profile* souboru.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-179">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="3b0a1-180">Upravte soubor pomocí libovolného textového editoru a přidejte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-180">Edit the file using any text editor and add the following statment.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="3b0a1-181">Linux</span><span class="sxs-lookup"><span data-stu-id="3b0a1-181">Linux</span></span>
<span data-ttu-id="3b0a1-182">Pro distribucích systému Linux, použijte `export` příkaz na příkazovém řádku pro relace na základě nastavení proměnné a *bash_profile* soubor pro nastavení úrovně prostředí počítače.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-182">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="3b0a1-183">Konfigurace prostředí</span><span class="sxs-lookup"><span data-stu-id="3b0a1-183">Configuration by environment</span></span>

<span data-ttu-id="3b0a1-184">V tématu [konfigurace prostředí](xref:fundamentals/configuration/index#configuration-by-environment) Další informace.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-184">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="3b0a1-185">Prostředí na základě spuštění třídy a metody</span><span class="sxs-lookup"><span data-stu-id="3b0a1-185">Environment based Startup class and methods</span></span>

<span data-ttu-id="3b0a1-186">Při spuštění aplikace ASP.NET Core [třída při spuštění](xref:fundamentals/startup) bootstraps aplikace.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-186">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="3b0a1-187">Pokud se třída `Startup{EnvironmentName}` existuje, že třídu bude volat třída pro tento `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="3b0a1-187">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="3b0a1-188">Poznámka: Volání [WebHostBuilder.UseStartup<TStartup> ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) přepsání konfigurační oddíly.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-188">Note: Calling [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="3b0a1-189">[Konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) podporu prostředí konkrétních verzích formuláře `Configure{EnvironmentName}` a `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="3b0a1-189">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="3b0a1-190">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3b0a1-190">Additional resources</span></span>

* [<span data-ttu-id="3b0a1-191">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="3b0a1-191">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="3b0a1-192">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="3b0a1-192">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="3b0a1-193">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="3b0a1-193">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
