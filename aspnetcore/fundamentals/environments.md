---
title: Použití prostředí s více v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak můžete řídit chování aplikace v prostředí s více v aplikacích ASP.NET Core.
ms.author: riande
ms.date: 06/21/2018
uid: fundamentals/environments
ms.openlocfilehash: 505f19d8b4df6e476b46a1fe7c49872d3c4acc1a
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314101"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="1d769-103">Použití prostředí s více v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1d769-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="1d769-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1d769-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1d769-105">ASP.NET Core nakonfiguruje chování aplikace založené na prostředí runtime pomocí proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="1d769-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="1d769-106">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1d769-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="1d769-107">Prostředí</span><span class="sxs-lookup"><span data-stu-id="1d769-107">Environments</span></span>

<span data-ttu-id="1d769-108">Přečte proměnné prostředí ASP.NET Core `ASPNETCORE_ENVIRONMENT` při spuštění aplikace a ukládá hodnotu v [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span><span class="sxs-lookup"><span data-stu-id="1d769-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span></span> <span data-ttu-id="1d769-109">Můžete nastavit `ASPNETCORE_ENVIRONMENT` na jakoukoli hodnotu, ale [tří hodnot](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) podporuje rozhraní: [vývoj](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [pracovní](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), a [produkční](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span><span class="sxs-lookup"><span data-stu-id="1d769-109">You can set `ASPNETCORE_ENVIRONMENT` to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span></span> <span data-ttu-id="1d769-110">Pokud `ASPNETCORE_ENVIRONMENT` není nastaven, nastaví se jako výchozí `Production`.</span><span class="sxs-lookup"><span data-stu-id="1d769-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it defaults to `Production`.</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="1d769-111">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="1d769-111">The preceding code:</span></span>

* <span data-ttu-id="1d769-112">Volání [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) a [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) při `ASPNETCORE_ENVIRONMENT` je nastaven na `Development`.</span><span class="sxs-lookup"><span data-stu-id="1d769-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="1d769-113">Volání [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) při hodnotě `ASPNETCORE_ENVIRONMENT` je nastavit jednu z následujících:</span><span class="sxs-lookup"><span data-stu-id="1d769-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="1d769-114">[Pomocná značku prostředí](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) používá hodnotu `IHostingEnvironment.EnvironmentName` pro zahrnutí nebo vyloučení značek v elementu:</span><span class="sxs-lookup"><span data-stu-id="1d769-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="1d769-115">V systémech Windows a systému macOS nejsou proměnné prostředí a hodnoty velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="1d769-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="1d769-116">Proměnné prostředí Linux a jejich hodnoty jsou **velká a malá písmena** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="1d769-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="1d769-117">Vývoj</span><span class="sxs-lookup"><span data-stu-id="1d769-117">Development</span></span>

<span data-ttu-id="1d769-118">Vývojové prostředí můžete povolit funkce, které by neměly být vystaveny v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1d769-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="1d769-119">Například šablony ASP.NET Core povolit [vývojáře výjimka stránky](xref:fundamentals/error-handling#the-developer-exception-page) ve vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="1d769-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="1d769-120">Prostředí pro vývoj na místním počítači může být nastavena v *Properties\launchSettings.json* souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="1d769-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="1d769-121">Hodnoty prostředí nastavené *launchSettings.json* přepsat hodnoty nastavené v prostředí systému.</span><span class="sxs-lookup"><span data-stu-id="1d769-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="1d769-122">Zobrazuje tři profily z následujícím kódu JSON *launchSettings.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="1d769-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="1d769-123">`applicationUrl` Vlastnost *launchSettings.json* můžete zadat seznam adres URL serveru.</span><span class="sxs-lookup"><span data-stu-id="1d769-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="1d769-124">Použijte středník mezi adresy URL v seznamu:</span><span class="sxs-lookup"><span data-stu-id="1d769-124">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="1d769-125">Při spuštění aplikace s [dotnet spustit](/dotnet/core/tools/dotnet-run), první profil s `"commandName": "Project"` se používá.</span><span class="sxs-lookup"><span data-stu-id="1d769-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="1d769-126">Hodnota `commandName` určuje webový server ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="1d769-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="1d769-127">`commandName` může být některého z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="1d769-127">`commandName` can be any one of the following:</span></span>

* <span data-ttu-id="1d769-128">Služby IIS Express</span><span class="sxs-lookup"><span data-stu-id="1d769-128">IIS Express</span></span>
* <span data-ttu-id="1d769-129">IIS</span><span class="sxs-lookup"><span data-stu-id="1d769-129">IIS</span></span>
* <span data-ttu-id="1d769-130">Projekt (který se spouští Kestrel)</span><span class="sxs-lookup"><span data-stu-id="1d769-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="1d769-131">Když se aplikace spustí s [dotnet spustit](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="1d769-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="1d769-132">*launchSettings.json* je pro čtení. Pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="1d769-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="1d769-133">`environmentVariables` nastavení v *launchSettings.json* přepsat proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="1d769-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="1d769-134">Hostování prostředí bude zobrazeno.</span><span class="sxs-lookup"><span data-stu-id="1d769-134">The hosting environment is displayed.</span></span>

<span data-ttu-id="1d769-135">Následující výstup zobrazuje aplikace spuštěna s [dotnet spustit](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="1d769-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="1d769-136">Visual Studio **ladění** karta poskytuje grafickým uživatelským rozhraním upravit *launchSettings.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="1d769-136">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proměnné prostředí nastavení vlastností projektu](environments/_static/project-properties-debug.png)

<span data-ttu-id="1d769-138">Změny provedené v projektu profily pravděpodobně projeví až po restartu webového serveru.</span><span class="sxs-lookup"><span data-stu-id="1d769-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="1d769-139">Kestrel může zjistit změny provedené v jeho prostředí musí být restartován.</span><span class="sxs-lookup"><span data-stu-id="1d769-139">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="1d769-140">*launchSettings.json* neměli ukládat tajné klíče.</span><span class="sxs-lookup"><span data-stu-id="1d769-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="1d769-141">[Nástroj tajný klíč správce](xref:security/app-secrets) slouží k uložení tajné klíče pro místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="1d769-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="1d769-142">Při použití [Visual Studio Code](https://code.visualstudio.com/), proměnné prostředí může být nastavena v *.vscode/launch.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="1d769-142">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="1d769-143">Následující příklad nastaví prostředí `Development`:</span><span class="sxs-lookup"><span data-stu-id="1d769-143">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="1d769-144">A *.vscode/launch.json* soubor v projektu není přečíst při spuštění aplikace s `dotnet run` stejným způsobem jako *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1d769-144">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="1d769-145">Při spuštění aplikace v vývoj, který nemá *launchSettings.json* souboru nastavená buď v prostředí s proměnné prostředí nebo argument příkazového řádku k `dotnet run` příkaz.</span><span class="sxs-lookup"><span data-stu-id="1d769-145">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="1d769-146">Produkční</span><span class="sxs-lookup"><span data-stu-id="1d769-146">Production</span></span>

<span data-ttu-id="1d769-147">Maximalizovat zabezpečení, výkonu a odolnosti aplikace musí být nakonfigurovaný v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1d769-147">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="1d769-148">Některé běžné nastavení, které se liší od vývoj patří:</span><span class="sxs-lookup"><span data-stu-id="1d769-148">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="1d769-149">Ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1d769-149">Caching.</span></span>
* <span data-ttu-id="1d769-150">Prostředky na straně klienta jsou seskupeny, minifikovaný a potenciálně zpracovat z název CDN.</span><span class="sxs-lookup"><span data-stu-id="1d769-150">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="1d769-151">Diagnostické chybové stránky zakázána.</span><span class="sxs-lookup"><span data-stu-id="1d769-151">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="1d769-152">Povolit popisný chybové stránky.</span><span class="sxs-lookup"><span data-stu-id="1d769-152">Friendly error pages enabled.</span></span>
* <span data-ttu-id="1d769-153">Produkční protokolování a monitorování povoleno.</span><span class="sxs-lookup"><span data-stu-id="1d769-153">Production logging and monitoring enabled.</span></span> <span data-ttu-id="1d769-154">Například [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="1d769-154">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="1d769-155">Nastavení prostředí</span><span class="sxs-lookup"><span data-stu-id="1d769-155">Setting the environment</span></span>

<span data-ttu-id="1d769-156">Často je vhodné nastavit konkrétní prostředí pro testování.</span><span class="sxs-lookup"><span data-stu-id="1d769-156">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="1d769-157">Pokud není nastavena v prostředí, je standardně `Production`, která zakáže většinu funkcí ladění.</span><span class="sxs-lookup"><span data-stu-id="1d769-157">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="1d769-158">Metoda pro nastavení prostředí závisí na operačním systému.</span><span class="sxs-lookup"><span data-stu-id="1d769-158">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="1d769-159">Aplikační služba Azure</span><span class="sxs-lookup"><span data-stu-id="1d769-159">Azure App Service</span></span>

<span data-ttu-id="1d769-160">Nastavení prostředí v [Azure App Service](https://azure.microsoft.com/services/app-service/), proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1d769-160">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="1d769-161">Vyberte si aplikaci z **App Services** okno.</span><span class="sxs-lookup"><span data-stu-id="1d769-161">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="1d769-162">V **nastavení** skupiny, vyberte **nastavení aplikace** okno.</span><span class="sxs-lookup"><span data-stu-id="1d769-162">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="1d769-163">V **nastavení aplikace** oblasti, vyberte **přidejte nové nastavení**.</span><span class="sxs-lookup"><span data-stu-id="1d769-163">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="1d769-164">Pro **zadejte název**, poskytovat `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="1d769-164">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="1d769-165">Pro **zadejte hodnotu**, poskytovat prostředí (například `Staging`).</span><span class="sxs-lookup"><span data-stu-id="1d769-165">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="1d769-166">Vyberte **nastavení slotu** zaškrtávací políčko, pokud chcete nastavení prostředí zůstat s aktuální pozici, když jsou vzájemně zaměněny nasazovací sloty.</span><span class="sxs-lookup"><span data-stu-id="1d769-166">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="1d769-167">Další informace najdete v tématu [dokumentaci k Azure: nastavení, které jsou vzájemně zaměněny?](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="1d769-167">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="1d769-168">Vyberte **Uložit** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="1d769-168">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="1d769-169">Aplikační služba Azure aplikace automaticky restartuje po nastavení aplikace (proměnnou prostředí) je přidat, změnit ani odstranit na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1d769-169">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

### <a name="windows"></a><span data-ttu-id="1d769-170">Windows</span><span class="sxs-lookup"><span data-stu-id="1d769-170">Windows</span></span>

<span data-ttu-id="1d769-171">Chcete-li nastavit `ASPNETCORE_ENVIRONMENT` pro aktuální relaci při spuštění aplikace pomocí [dotnet spustit](/dotnet/core/tools/dotnet-run), se používají následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1d769-171">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="1d769-172">**Příkazový řádek**</span><span class="sxs-lookup"><span data-stu-id="1d769-172">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="1d769-173">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="1d769-173">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="1d769-174">Tyto příkazy projeví u aktuálního okna.</span><span class="sxs-lookup"><span data-stu-id="1d769-174">These commands only take effect for the current window.</span></span> <span data-ttu-id="1d769-175">Při zavření okna `ASPNETCORE_ENVIRONMENT` bude obnoveno výchozí nastavení nebo hodnota počítače.</span><span class="sxs-lookup"><span data-stu-id="1d769-175">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="1d769-176">Chcete-li nastavit hodnotu globálně v systému Windows, otevřete **ovládací panely** > **systému** > **Upřesnit nastavení systému** a přidat nebo upravit `ASPNETCORE_ENVIRONMENT`hodnotu:</span><span class="sxs-lookup"><span data-stu-id="1d769-176">To set the value globally in Windows, open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

![Rozšířené vlastnosti systému](environments/_static/systemsetting_environment.png)

![Proměnné prostředí základní ASPNET](environments/_static/windows_aspnetcore_environment.png)

<span data-ttu-id="1d769-179">**web.config**</span><span class="sxs-lookup"><span data-stu-id="1d769-179">**web.config**</span></span>

<span data-ttu-id="1d769-180">Najdete v článku *nastavení proměnných prostředí* části [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tématu.</span><span class="sxs-lookup"><span data-stu-id="1d769-180">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="1d769-181">**Fondy aplikací služby IIS**</span><span class="sxs-lookup"><span data-stu-id="1d769-181">**Per IIS Application Pool**</span></span>

<span data-ttu-id="1d769-182">Nastavení proměnných prostředí pro jednotlivé aplikace běžící v izolované fondy aplikací (podporuje se ve službě IIS 10.0 +), najdete v článku *příkazu AppCmd.exe* části [proměnné prostředí &lt; environmentVariables&gt; ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tématu.</span><span class="sxs-lookup"><span data-stu-id="1d769-182">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="1d769-183">macOS</span><span class="sxs-lookup"><span data-stu-id="1d769-183">macOS</span></span>

<span data-ttu-id="1d769-184">Nastavení aktuální prostředí může být systému macOS prováděné v řádku, při kterém aplikace běží:</span><span class="sxs-lookup"><span data-stu-id="1d769-184">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="1d769-185">Alternativně nastavte prostředí s `export` před spuštěním aplikace:</span><span class="sxs-lookup"><span data-stu-id="1d769-185">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="1d769-186">Proměnné prostředí v úrovni počítače jsou nastavené *.bashrc* nebo *.bash_profile* souboru.</span><span class="sxs-lookup"><span data-stu-id="1d769-186">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="1d769-187">Upravte soubor pomocí libovolného textového editoru.</span><span class="sxs-lookup"><span data-stu-id="1d769-187">Edit the file using any text editor.</span></span> <span data-ttu-id="1d769-188">Přidejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1d769-188">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="1d769-189">Linux</span><span class="sxs-lookup"><span data-stu-id="1d769-189">Linux</span></span>

<span data-ttu-id="1d769-190">Pro distribucích systému Linux, použijte `export` příkazu na příkazovém řádku pro nastavení proměnné na bázi relací a *bash_profile* soubor pro nastavení prostředí úrovni počítače.</span><span class="sxs-lookup"><span data-stu-id="1d769-190">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="1d769-191">Konfigurace prostředí</span><span class="sxs-lookup"><span data-stu-id="1d769-191">Configuration by environment</span></span>

<span data-ttu-id="1d769-192">V tématu [konfigurace prostředí](xref:fundamentals/configuration/index#configuration-by-environment) Další informace.</span><span class="sxs-lookup"><span data-stu-id="1d769-192">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="1d769-193">Třídy založené na prostředí pro spuštění a metody</span><span class="sxs-lookup"><span data-stu-id="1d769-193">Environment-based Startup class and methods</span></span>

<span data-ttu-id="1d769-194">Při spuštění aplikace ASP.NET Core [třída při spuštění](xref:fundamentals/startup) bootstraps aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d769-194">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="1d769-195">Pokud `Startup{EnvironmentName}` třída existuje, pro který se nazývá třída `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="1d769-195">If a `Startup{EnvironmentName}` class exists, the class is called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="1d769-196">[WebHostBuilder.UseStartup<TStartup> ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) přepsání konfigurační oddíly.</span><span class="sxs-lookup"><span data-stu-id="1d769-196">[WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="1d769-197">[Konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) podporují konkrétní prostředí verzích formuláře `Configure{EnvironmentName}` a `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="1d769-197">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="1d769-198">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1d769-198">Additional resources</span></span>

* [<span data-ttu-id="1d769-199">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="1d769-199">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="1d769-200">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="1d769-200">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="1d769-201">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="1d769-201">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
