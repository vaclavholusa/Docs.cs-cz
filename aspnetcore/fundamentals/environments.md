---
title: Používání více prostředí v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak řídit chování aplikace napříč několika prostředími v aplikacích ASP.NET Core.
ms.author: riande
ms.date: 07/03/2018
uid: fundamentals/environments
ms.openlocfilehash: 8983a0ce81beb16d68c799d30bfbfce6e7b693b1
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433945"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="6985d-103">Používání více prostředí v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6985d-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="6985d-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6985d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6985d-105">ASP.NET Core nakonfiguruje chování aplikace založené na prostředí modulu runtime pomocí proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="6985d-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="6985d-106">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6985d-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="6985d-107">Prostředí</span><span class="sxs-lookup"><span data-stu-id="6985d-107">Environments</span></span>

<span data-ttu-id="6985d-108">ASP.NET Core načte proměnnou prostředí `ASPNETCORE_ENVIRONMENT` při spuštění aplikace a uloží hodnotu v [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span><span class="sxs-lookup"><span data-stu-id="6985d-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span></span> <span data-ttu-id="6985d-109">Můžete nastavit `ASPNETCORE_ENVIRONMENT` na libovolnou hodnotu, ale [tří hodnot](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) jsou podporovány v rámci rozhraní: [vývoj](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [pracovní](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), a [produkční](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span><span class="sxs-lookup"><span data-stu-id="6985d-109">You can set `ASPNETCORE_ENVIRONMENT` to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span></span> <span data-ttu-id="6985d-110">Pokud `ASPNETCORE_ENVIRONMENT` není nastavený, použije se výchozí `Production`.</span><span class="sxs-lookup"><span data-stu-id="6985d-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it defaults to `Production`.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="6985d-111">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="6985d-111">The preceding code:</span></span>

* <span data-ttu-id="6985d-112">Volání [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) a [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) při `ASPNETCORE_ENVIRONMENT` je nastavena na `Development`.</span><span class="sxs-lookup"><span data-stu-id="6985d-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="6985d-113">Volání [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) při hodnotu `ASPNETCORE_ENVIRONMENT` nastavený jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="6985d-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="6985d-114">[Pomocná rutina značky prostředí](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) používá hodnotu `IHostingEnvironment.EnvironmentName` pro zahrnutí nebo vyloučení značek v elementu:</span><span class="sxs-lookup"><span data-stu-id="6985d-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="6985d-115">Ve Windows a macOS proměnné prostředí a hodnoty nejsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="6985d-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="6985d-116">Proměnné prostředí Linux a hodnoty jsou **malá a velká písmena** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="6985d-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="6985d-117">Vývoj</span><span class="sxs-lookup"><span data-stu-id="6985d-117">Development</span></span>

<span data-ttu-id="6985d-118">Vývojové prostředí můžete povolit funkce, které by neměly být vystaveny v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6985d-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="6985d-119">Například šablony ASP.NET Core povolit [stránku výjimek pro vývojáře](xref:fundamentals/error-handling#the-developer-exception-page) ve vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="6985d-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="6985d-120">Je možné nastavit prostředí pro vývoj v místním počítači v *Properties\launchSettings.json* souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="6985d-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="6985d-121">Hodnoty prostředí nastavené *launchSettings.json* přepisují hodnoty nastavené v prostředí systému.</span><span class="sxs-lookup"><span data-stu-id="6985d-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="6985d-122">Následující kód JSON ukazuje tři profily *launchSettings.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="6985d-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="6985d-123">`applicationUrl` Vlastnost *launchSettings.json* můžete zadat seznam adres URL serveru.</span><span class="sxs-lookup"><span data-stu-id="6985d-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="6985d-124">Použijte středník mezi adresy URL v seznamu:</span><span class="sxs-lookup"><span data-stu-id="6985d-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

::: moniker-end

<span data-ttu-id="6985d-125">Při spuštění aplikace s [dotnet spustit](/dotnet/core/tools/dotnet-run), první profil s `"commandName": "Project"` se používá.</span><span class="sxs-lookup"><span data-stu-id="6985d-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="6985d-126">Hodnota `commandName` specifikuje webový server ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="6985d-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="6985d-127">`commandName` může být jedna z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="6985d-127">`commandName` can be any one of the following:</span></span>

* <span data-ttu-id="6985d-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="6985d-128">IIS Express</span></span>
* <span data-ttu-id="6985d-129">IIS</span><span class="sxs-lookup"><span data-stu-id="6985d-129">IIS</span></span>
* <span data-ttu-id="6985d-130">Projekt (které spustí Kestrel)</span><span class="sxs-lookup"><span data-stu-id="6985d-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="6985d-131">Když je aplikace spuštěna s [dotnet spustit](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="6985d-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="6985d-132">*launchSettings.json* je pro čtení. Pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="6985d-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="6985d-133">`environmentVariables` nastavení v *launchSettings.json* přepsání proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="6985d-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="6985d-134">Hostitelské prostředí se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="6985d-134">The hosting environment is displayed.</span></span>

<span data-ttu-id="6985d-135">Následující výstup zobrazuje aplikaci začít [dotnet spustit](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="6985d-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="6985d-136">Vlastnosti projektu sady Visual Studio **ladění** karta poskytuje grafické uživatelské rozhraní pro úpravy *launchSettings.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="6985d-136">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proměnné prostředí nastavení vlastností projektu](environments/_static/project-properties-debug.png)

<span data-ttu-id="6985d-138">Změny provedené v projektu profily pravděpodobně projeví až po restartu webového serveru.</span><span class="sxs-lookup"><span data-stu-id="6985d-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="6985d-139">Kestrel je nutné restartovat předtím, než může zjistit změny provedené v prostředí.</span><span class="sxs-lookup"><span data-stu-id="6985d-139">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="6985d-140">*launchSettings.json* neměli ukládat tajné kódy.</span><span class="sxs-lookup"><span data-stu-id="6985d-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="6985d-141">[Nástroj tajný klíč správce](xref:security/app-secrets) slouží k ukládání tajných kódů pro místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="6985d-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="6985d-142">Při použití [Visual Studio Code](https://code.visualstudio.com/), proměnné prostředí můžete nastavit v *.vscode/launch.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="6985d-142">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="6985d-143">Následující příklad nastaví prostředí `Development`:</span><span class="sxs-lookup"><span data-stu-id="6985d-143">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="6985d-144">A *.vscode/launch.json* soubor v projektu pro není čtení při spuštění aplikace s `dotnet run` stejným způsobem jako *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6985d-144">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="6985d-145">Zobrazit *konfigurace podle prostředí* Další informace.</span><span class="sxs-lookup"><span data-stu-id="6985d-145">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="6985d-146">Produkční</span><span class="sxs-lookup"><span data-stu-id="6985d-146">Production</span></span>

<span data-ttu-id="6985d-147">Na základě prostředí při spuštění třídy a metody</span><span class="sxs-lookup"><span data-stu-id="6985d-147">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="6985d-148">Při spuštění aplikace ASP.NET Core třídu pro spuštění bootstraps aplikace.</span><span class="sxs-lookup"><span data-stu-id="6985d-148">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="6985d-149">Pokud  existuje třída, pro, který se nazývá třídy :</span><span class="sxs-lookup"><span data-stu-id="6985d-149">Caching.</span></span>
* <span data-ttu-id="6985d-150">WebHostBuilder.UseStartupTStartup  přepíše konfigurační oddíly funkce.</span><span class="sxs-lookup"><span data-stu-id="6985d-150">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="6985d-151">Konfigurace a ConfigureServices podpory verzí specifických pro prostředí formu  a :</span><span class="sxs-lookup"><span data-stu-id="6985d-151">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="6985d-152">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="6985d-152">Friendly error pages enabled.</span></span>
* <span data-ttu-id="6985d-153">Produkční protokolování a monitorování povoleno.</span><span class="sxs-lookup"><span data-stu-id="6985d-153">Production logging and monitoring enabled.</span></span> <span data-ttu-id="6985d-154">Například [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="6985d-154">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="6985d-155">Nastavení prostředí</span><span class="sxs-lookup"><span data-stu-id="6985d-155">Set the environment</span></span>

<span data-ttu-id="6985d-156">Často je užitečné k nastavení konkrétního prostředí pro testování.</span><span class="sxs-lookup"><span data-stu-id="6985d-156">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="6985d-157">Pokud prostředí není nastavená, použije se výchozí `Production`, která zakáže většina funkcí ladění.</span><span class="sxs-lookup"><span data-stu-id="6985d-157">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="6985d-158">Metoda pro nastavení prostředí závisí na operačním systému.</span><span class="sxs-lookup"><span data-stu-id="6985d-158">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="6985d-159">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6985d-159">Azure App Service</span></span>

<span data-ttu-id="6985d-160">Chcete-li nastavit prostředí [služby Azure App Service](https://azure.microsoft.com/services/app-service/), proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6985d-160">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="6985d-161">Vyberte aplikaci, ze **App Services** okno.</span><span class="sxs-lookup"><span data-stu-id="6985d-161">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="6985d-162">V **nastavení** skupiny, vyberte **nastavení aplikace** okno.</span><span class="sxs-lookup"><span data-stu-id="6985d-162">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="6985d-163">V **nastavení aplikace** vyberte **přidat nové nastavení**.</span><span class="sxs-lookup"><span data-stu-id="6985d-163">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="6985d-164">Pro **zadejte název**, poskytují `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="6985d-164">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="6985d-165">Pro **zadejte hodnotu**, poskytovat prostředí (například `Staging`).</span><span class="sxs-lookup"><span data-stu-id="6985d-165">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="6985d-166">Vyberte **nastavení slotu** zaškrtávací políčko, pokud chcete nastavení prostředí, které zůstane s aktuálním slotu, když se Prohodit sloty nasazení.</span><span class="sxs-lookup"><span data-stu-id="6985d-166">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="6985d-167">Další informace najdete v tématu [dokumentaci k Azure: nastavení, které jsou přehozeny?](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="6985d-167">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="6985d-168">Vyberte **Uložit** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="6985d-168">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="6985d-169">Po nastavení aplikace (proměnnou prostředí) přidání, změně nebo odstranění na webu Azure Portal, Azure App Service automaticky restartuje aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6985d-169">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

### <a name="windows"></a><span data-ttu-id="6985d-170">Windows</span><span class="sxs-lookup"><span data-stu-id="6985d-170">Windows</span></span>

<span data-ttu-id="6985d-171">Chcete-li nastavit `ASPNETCORE_ENVIRONMENT` pro aktuální relaci při spuštění aplikace pomocí [dotnet spustit](/dotnet/core/tools/dotnet-run), se používají následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="6985d-171">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="6985d-172">**Příkazový řádek**</span><span class="sxs-lookup"><span data-stu-id="6985d-172">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="6985d-173">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="6985d-173">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="6985d-174">Tyto příkazy platit jenom pro aktuální okno.</span><span class="sxs-lookup"><span data-stu-id="6985d-174">These commands only take effect for the current window.</span></span> <span data-ttu-id="6985d-175">Při zavření okna `ASPNETCORE_ENVIRONMENT` nastavení obnoví na výchozí nastavení nebo počítač hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6985d-175">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="6985d-176">Chcete-li nastavit hodnotu globálně ve Windows, otevřete **ovládací panely** > **systému** > **Upřesnit nastavení systému** a přidat nebo upravit `ASPNETCORE_ENVIRONMENT`hodnotu:</span><span class="sxs-lookup"><span data-stu-id="6985d-176">To set the value globally in Windows, open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

![Systémové rozšířené vlastnosti](environments/_static/systemsetting_environment.png)

![Proměnná prostředí základní ASPNET](environments/_static/windows_aspnetcore_environment.png)

<span data-ttu-id="6985d-179">**web.config**</span><span class="sxs-lookup"><span data-stu-id="6985d-179">**web.config**</span></span>

<span data-ttu-id="6985d-180">Najdete v článku *nastavení proměnných prostředí* část [odkaz Konfigurace modul ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tématu.</span><span class="sxs-lookup"><span data-stu-id="6985d-180">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="6985d-181">**Na jeden fond aplikací služby IIS**</span><span class="sxs-lookup"><span data-stu-id="6985d-181">**Per IIS Application Pool**</span></span>

<span data-ttu-id="6985d-182">Nastavení proměnných prostředí pro jednotlivé aplikace spuštěné v izolované fondy aplikací (podporované ve službě IIS 10.0 a novější), najdete v článku *AppCmd.exe příkaz* část [proměnné prostředí &lt; environmentVariables&gt; ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tématu.</span><span class="sxs-lookup"><span data-stu-id="6985d-182">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="6985d-183">macOS</span><span class="sxs-lookup"><span data-stu-id="6985d-183">macOS</span></span>

<span data-ttu-id="6985d-184">Nastavení aktuální prostředí pro macOS může být prováděny v řádku při spuštění aplikace:</span><span class="sxs-lookup"><span data-stu-id="6985d-184">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="6985d-185">Můžete také nastavit prostředí pomocí `export` před spuštěním aplikace:</span><span class="sxs-lookup"><span data-stu-id="6985d-185">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="6985d-186">Proměnné prostředí na úrovni počítače se nastavují *.bashrc* nebo *.bash_profile* souboru.</span><span class="sxs-lookup"><span data-stu-id="6985d-186">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="6985d-187">Upravte soubor pomocí libovolného textového editoru.</span><span class="sxs-lookup"><span data-stu-id="6985d-187">Edit the file using any text editor.</span></span> <span data-ttu-id="6985d-188">Přidejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6985d-188">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="6985d-189">Linux</span><span class="sxs-lookup"><span data-stu-id="6985d-189">Linux</span></span>

<span data-ttu-id="6985d-190">Pro distribuce Linuxu, použijte `export` příkazu na příkazovém řádku pro nastavení proměnné na základě relace a *bash_profile* souboru pro nastavení prostředí na úrovni počítače.</span><span class="sxs-lookup"><span data-stu-id="6985d-190">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="6985d-191">Konfigurace podle prostředí</span><span class="sxs-lookup"><span data-stu-id="6985d-191">Configuration by environment</span></span>

<span data-ttu-id="6985d-192">Zobrazit [konfigurace podle prostředí](xref:fundamentals/configuration/index#configuration-by-environment) Další informace.</span><span class="sxs-lookup"><span data-stu-id="6985d-192">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="6985d-193">Na základě prostředí při spuštění třídy a metody</span><span class="sxs-lookup"><span data-stu-id="6985d-193">Environment-based Startup class and methods</span></span>

<span data-ttu-id="6985d-194">Při spuštění aplikace ASP.NET Core [třídu pro spuštění](xref:fundamentals/startup) bootstraps aplikace.</span><span class="sxs-lookup"><span data-stu-id="6985d-194">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="6985d-195">Pokud `Startup{EnvironmentName}` existuje třída, pro, který se nazývá třídy `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="6985d-195">If a `Startup{EnvironmentName}` class exists, the class is called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="6985d-196">[WebHostBuilder.UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) přepíše konfigurační oddíly funkce.</span><span class="sxs-lookup"><span data-stu-id="6985d-196">[WebHostBuilder.UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="6985d-197">[Konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) podpory verzí specifických pro prostředí formu `Configure{EnvironmentName}` a `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="6985d-197">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a><span data-ttu-id="6985d-198">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6985d-198">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="6985d-199">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="6985d-199">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
