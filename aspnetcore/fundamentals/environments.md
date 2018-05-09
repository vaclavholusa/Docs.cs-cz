---
title: Použití prostředí s více v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak ASP.NET Core poskytuje podporu pro řízení chování aplikace ve více prostředích.
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: 2c8441db527203aeea516073dae3bc335c335565
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="use-multiple-environments-in-aspnet-core"></a>Použití prostředí s více v ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Základní technologie ASP.NET poskytuje podporu pro nastavení chování aplikace za běhu pomocí proměnné prostředí.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Prostředí

Přečte proměnné prostředí ASP.NET Core `ASPNETCORE_ENVIRONMENT` při spuštění aplikace a úložišť, která hodnotu [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName). `ASPNETCORE_ENVIRONMENT` můžete nastavit na jakoukoli hodnotu, ale [tří hodnot](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) podporuje rozhraní: [vývoj](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [pracovní](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), a [produkční](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0). Pokud `ASPNETCORE_ENVIRONMENT` není nastaven, bude použita výchozí `Production`.

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

Předchozí kód:

* Volání [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) a [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) při `ASPNETCORE_ENVIRONMENT` je nastaven na `Development`.
* Volání [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) při hodnotě `ASPNETCORE_ENVIRONMENT` je nastavit jednu z následujících:

    * `Staging`
    * `Production`
    * `Staging_2`

[Pomocná značku prostředí ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) používá hodnotu `IHostingEnvironment.EnvironmentName` pro zahrnutí nebo vyloučení značek v elementu:

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

Poznámka: V systémech Windows a systému macOS proměnné prostředí a hodnoty nejsou velká a malá písmena. Proměnné prostředí Linux a jejich hodnoty jsou **velká a malá písmena** ve výchozím nastavení.

### <a name="development"></a>Vývoj

Vývojové prostředí můžete povolit funkce, které by neměly být vystaveny v produkčním prostředí. Například šablony ASP.NET Core povolit [vývojáře výjimka stránky](xref:fundamentals/error-handling#the-developer-exception-page) ve vývojovém prostředí.

Prostředí pro vývoj na místním počítači může být nastavena v *Properties\launchSettings.json* souboru projektu. Hodnoty prostředí nastavené *launchSettings.json* přepsat hodnoty nastavené v prostředí systému.

Zobrazuje tři profily z následujícím kódu JSON *launchSettings.json* souboru:

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> `applicationUrl` Vlastnost *launchSettings.json* můžete zadat seznam adres URL serveru. Použijte středník mezi adresy URL v seznamu:
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

Když je aplikace spuštěna s [dotnet spustit](/dotnet/core/tools/dotnet-run), první profil s `"commandName": "Project"` se použije. Hodnota `commandName` určuje webový server ke spuštění. `commandName` může být jedna z:

* Služby IIS Express
* IIS
* Projekt (který se spouští Kestrel)

Když se aplikace spustí s [dotnet spustit](/dotnet/core/tools/dotnet-run):

* *launchSettings.json* je pro čtení. Pokud je k dispozici. `environmentVariables` nastavení v *launchSettings.json* přepsat proměnné prostředí.
* Hostování prostředí bude zobrazeno.


Následující výstup zobrazuje aplikace spuštěna s [dotnet spustit](/dotnet/core/tools/dotnet-run):
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Visual Studio **ladění** karta poskytuje grafickým uživatelským rozhraním upravit *launchSettings.json* souboru:

![Proměnné prostředí nastavení vlastností projektu](environments/_static/project-properties-debug.png)

Změny provedené v projektu profily pravděpodobně projeví až po restartu webového serveru. Kestrel zjistí změny provedené v jeho prostředí musí být restartován.

>[!WARNING]
> *launchSettings.json* neměli ukládat tajné klíče. [Nástroj tajný klíč správce](xref:security/app-secrets) slouží k uložení tajné klíče pro místní vývoj.

### <a name="production"></a>Produkční

Maximalizovat zabezpečení, výkonu a odolnosti aplikace musí být nakonfigurovaný v provozním prostředí. Některé běžné nastavení, které se liší od vývoj patří:

* Ukládání do mezipaměti.
* Prostředky na straně klienta jsou seskupeny, minifikovaný a potenciálně zpracovat z název CDN.
* Diagnostické chybové stránky zakázána.
* Povolit popisný chybové stránky.
* Produkční protokolování a monitorování povoleno. Například [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="setting-the-environment"></a>Nastavení prostředí

Často je vhodné nastavit konkrétní prostředí pro testování. Pokud není nastavena v prostředí, bude výchozí `Production` který zakáže většinu funkcí ladění.

Metoda pro nastavení prostředí závisí na operačním systému.

### <a name="azure"></a>Azure

Pro službu Azure app service:

* Vyberte **nastavení aplikace** okno.
* Přidejte klíč a hodnotu **nastavení aplikace**.


### <a name="windows"></a>Windows
Chcete-li nastavit `ASPNETCORE_ENVIRONMENT` pro aktuální relaci, pokud je aplikace spuštěná pomocí [dotnet spustit](/dotnet/core/tools/dotnet-run), se používají následující příkazy

**Příkazový řádek**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Tyto příkazy se projeví pouze pro aktuální okno. Při zavření okna nastavení ASPNETCORE_ENVIRONMENT obnoví výchozí nastavení nebo počítač hodnotu. Chcete-li nastavit hodnotu globálně při otevření Windows **ovládací panely** > **systému** > **Upřesnit nastavení systému** a přidat nebo upravit `ASPNETCORE_ENVIRONMENT` hodnotu.

![Rozšířené vlastnosti systému](environments/_static/systemsetting_environment.png)

![Proměnné prostředí základní ASPNET](environments/_static/windows_aspnetcore_environment.png)


**web.config**

Najdete v článku *nastavení proměnných prostředí* části [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tématu.

**Fondy aplikací služby IIS**

Nastavení proměnných prostředí pro jednotlivé aplikace běžící v izolované fondy aplikací (podporuje se ve službě IIS 10.0 +), najdete v článku *příkazu AppCmd.exe* části [proměnné prostředí \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tématu.

### <a name="macos"></a>macOS
Nastavení aktuální prostředí pro systému macOS lze provést v řádku při spuštění aplikace;

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
nebo pomocí `export` pro ni nastavit před spuštěním aplikace.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
Proměnné prostředí úrovni počítače jsou nastaveny v *.bashrc* nebo *.bash_profile* souboru. Upravte soubor pomocí libovolného textového editoru a přidejte následující příkaz.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
Pro distribucích systému Linux, použijte `export` příkaz na příkazovém řádku pro relace na základě nastavení proměnné a *bash_profile* soubor pro nastavení úrovně prostředí počítače.

### <a name="configuration-by-environment"></a>Konfigurace prostředí

V tématu [konfigurace prostředí](xref:fundamentals/configuration/index#configuration-by-environment) Další informace.

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>Prostředí na základě spuštění třídy a metody

Při spuštění aplikace ASP.NET Core [třída při spuštění](xref:fundamentals/startup) bootstraps aplikace. Pokud se třída `Startup{EnvironmentName}` existuje, že třídu bude volat třída pro tento `EnvironmentName`:

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

Poznámka: Volání [WebHostBuilder.UseStartup<TStartup> ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) přepsání konfigurační oddíly.

[Konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) podporu prostředí konkrétních verzích formuláře `Configure{EnvironmentName}` a `Configure{EnvironmentName}Services`:

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>Další zdroje

* [Spuštění aplikace](xref:fundamentals/startup)
* [Konfigurace](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
