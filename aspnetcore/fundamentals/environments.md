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
# <a name="use-multiple-environments-in-aspnet-core"></a>Použití prostředí s více v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core nakonfiguruje chování aplikace založené na prostředí runtime pomocí proměnné prostředí.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Prostředí

Přečte proměnné prostředí ASP.NET Core `ASPNETCORE_ENVIRONMENT` při spuštění aplikace a ukládá hodnotu v [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname). Můžete nastavit `ASPNETCORE_ENVIRONMENT` na jakoukoli hodnotu, ale [tří hodnot](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) podporuje rozhraní: [vývoj](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [pracovní](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), a [produkční](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production). Pokud `ASPNETCORE_ENVIRONMENT` není nastaven, nastaví se jako výchozí `Production`.

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

Předchozí kód:

* Volání [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) a [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) při `ASPNETCORE_ENVIRONMENT` je nastaven na `Development`.
* Volání [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) při hodnotě `ASPNETCORE_ENVIRONMENT` je nastavit jednu z následujících:

    * `Staging`
    * `Production`
    * `Staging_2`

[Pomocná značku prostředí](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) používá hodnotu `IHostingEnvironment.EnvironmentName` pro zahrnutí nebo vyloučení značek v elementu:

[!code-cshtml[](environments/sample-snapshot/WebApp1/Pages/About.cshtml)]

V systémech Windows a systému macOS nejsou proměnné prostředí a hodnoty velká a malá písmena. Proměnné prostředí Linux a jejich hodnoty jsou **velká a malá písmena** ve výchozím nastavení.

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

Při spuštění aplikace s [dotnet spustit](/dotnet/core/tools/dotnet-run), první profil s `"commandName": "Project"` se používá. Hodnota `commandName` určuje webový server ke spuštění. `commandName` může být některého z následujících akcí:

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

Změny provedené v projektu profily pravděpodobně projeví až po restartu webového serveru. Kestrel může zjistit změny provedené v jeho prostředí musí být restartován.

> [!WARNING]
> *launchSettings.json* neměli ukládat tajné klíče. [Nástroj tajný klíč správce](xref:security/app-secrets) slouží k uložení tajné klíče pro místní vývoj.

Při použití [Visual Studio Code](https://code.visualstudio.com/), proměnné prostředí může být nastavena v *.vscode/launch.json* souboru. Následující příklad nastaví prostředí `Development`:

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

A *.vscode/launch.json* soubor v projektu není přečíst při spuštění aplikace s `dotnet run` stejným způsobem jako *Properties/launchSettings.json*. Při spuštění aplikace v vývoj, který nemá *launchSettings.json* souboru nastavená buď v prostředí s proměnné prostředí nebo argument příkazového řádku k `dotnet run` příkaz.

### <a name="production"></a>Produkční

Maximalizovat zabezpečení, výkonu a odolnosti aplikace musí být nakonfigurovaný v provozním prostředí. Některé běžné nastavení, které se liší od vývoj patří:

* Ukládání do mezipaměti.
* Prostředky na straně klienta jsou seskupeny, minifikovaný a potenciálně zpracovat z název CDN.
* Diagnostické chybové stránky zakázána.
* Povolit popisný chybové stránky.
* Produkční protokolování a monitorování povoleno. Například [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="setting-the-environment"></a>Nastavení prostředí

Často je vhodné nastavit konkrétní prostředí pro testování. Pokud není nastavena v prostředí, je standardně `Production`, která zakáže většinu funkcí ladění. Metoda pro nastavení prostředí závisí na operačním systému.

### <a name="azure-app-service"></a>Aplikační služba Azure

Nastavení prostředí v [Azure App Service](https://azure.microsoft.com/services/app-service/), proveďte následující kroky:

1. Vyberte si aplikaci z **App Services** okno.
1. V **nastavení** skupiny, vyberte **nastavení aplikace** okno.
1. V **nastavení aplikace** oblasti, vyberte **přidejte nové nastavení**.
1. Pro **zadejte název**, poskytovat `ASPNETCORE_ENVIRONMENT`. Pro **zadejte hodnotu**, poskytovat prostředí (například `Staging`).
1. Vyberte **nastavení slotu** zaškrtávací políčko, pokud chcete nastavení prostředí zůstat s aktuální pozici, když jsou vzájemně zaměněny nasazovací sloty. Další informace najdete v tématu [dokumentaci k Azure: nastavení, které jsou vzájemně zaměněny?](/azure/app-service/web-sites-staged-publishing).
1. Vyberte **Uložit** v horní části okna.

Aplikační služba Azure aplikace automaticky restartuje po nastavení aplikace (proměnnou prostředí) je přidat, změnit ani odstranit na portálu Azure.

### <a name="windows"></a>Windows

Chcete-li nastavit `ASPNETCORE_ENVIRONMENT` pro aktuální relaci při spuštění aplikace pomocí [dotnet spustit](/dotnet/core/tools/dotnet-run), se používají následující příkazy:

**Příkazový řádek**

```console
set ASPNETCORE_ENVIRONMENT=Development
```

**PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Tyto příkazy projeví u aktuálního okna. Při zavření okna `ASPNETCORE_ENVIRONMENT` bude obnoveno výchozí nastavení nebo hodnota počítače. Chcete-li nastavit hodnotu globálně v systému Windows, otevřete **ovládací panely** > **systému** > **Upřesnit nastavení systému** a přidat nebo upravit `ASPNETCORE_ENVIRONMENT`hodnotu:

![Rozšířené vlastnosti systému](environments/_static/systemsetting_environment.png)

![Proměnné prostředí základní ASPNET](environments/_static/windows_aspnetcore_environment.png)

**web.config**

Najdete v článku *nastavení proměnných prostředí* části [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tématu.

**Fondy aplikací služby IIS**

Nastavení proměnných prostředí pro jednotlivé aplikace běžící v izolované fondy aplikací (podporuje se ve službě IIS 10.0 +), najdete v článku *příkazu AppCmd.exe* části [proměnné prostředí &lt; environmentVariables&gt; ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tématu.

### <a name="macos"></a>macOS

Nastavení aktuální prostředí může být systému macOS prováděné v řádku, při kterém aplikace běží:

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

Alternativně nastavte prostředí s `export` před spuštěním aplikace:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

Proměnné prostředí v úrovni počítače jsou nastavené *.bashrc* nebo *.bash_profile* souboru. Upravte soubor pomocí libovolného textového editoru. Přidejte následující příkaz:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux

Pro distribucích systému Linux, použijte `export` příkazu na příkazovém řádku pro nastavení proměnné na bázi relací a *bash_profile* soubor pro nastavení prostředí úrovni počítače.

### <a name="configuration-by-environment"></a>Konfigurace prostředí

V tématu [konfigurace prostředí](xref:fundamentals/configuration/index#configuration-by-environment) Další informace.

## <a name="environment-based-startup-class-and-methods"></a>Třídy založené na prostředí pro spuštění a metody

Při spuštění aplikace ASP.NET Core [třída při spuštění](xref:fundamentals/startup) bootstraps aplikace. Pokud `Startup{EnvironmentName}` třída existuje, pro který se nazývá třída `EnvironmentName`:

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

[WebHostBuilder.UseStartup<TStartup> ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) přepsání konfigurační oddíly.

[Konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) podporují konkrétní prostředí verzích formuláře `Configure{EnvironmentName}` a `Configure{EnvironmentName}Services`:

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>Další zdroje

* [Spuštění aplikace](xref:fundamentals/startup)
* [Konfigurace](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
