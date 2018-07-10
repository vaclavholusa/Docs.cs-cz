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
# <a name="use-multiple-environments-in-aspnet-core"></a>Používání více prostředí v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core nakonfiguruje chování aplikace založené na prostředí modulu runtime pomocí proměnné prostředí.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Prostředí

ASP.NET Core načte proměnnou prostředí `ASPNETCORE_ENVIRONMENT` při spuštění aplikace a uloží hodnotu v [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname). Můžete nastavit `ASPNETCORE_ENVIRONMENT` na libovolnou hodnotu, ale [tří hodnot](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) jsou podporovány v rámci rozhraní: [vývoj](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [pracovní](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), a [produkční](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production). Pokud `ASPNETCORE_ENVIRONMENT` není nastavený, použije se výchozí `Production`.

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

Předchozí kód:

* Volání [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) a [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) při `ASPNETCORE_ENVIRONMENT` je nastavena na `Development`.
* Volání [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) při hodnotu `ASPNETCORE_ENVIRONMENT` nastavený jeden z následujících akcí:

    * `Staging`
    * `Production`
    * `Staging_2`

[Pomocná rutina značky prostředí](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) používá hodnotu `IHostingEnvironment.EnvironmentName` pro zahrnutí nebo vyloučení značek v elementu:

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

Ve Windows a macOS proměnné prostředí a hodnoty nejsou velká a malá písmena. Proměnné prostředí Linux a hodnoty jsou **malá a velká písmena** ve výchozím nastavení.

### <a name="development"></a>Vývoj

Vývojové prostředí můžete povolit funkce, které by neměly být vystaveny v produkčním prostředí. Například šablony ASP.NET Core povolit [stránku výjimek pro vývojáře](xref:fundamentals/error-handling#the-developer-exception-page) ve vývojovém prostředí.

Je možné nastavit prostředí pro vývoj v místním počítači v *Properties\launchSettings.json* souboru projektu. Hodnoty prostředí nastavené *launchSettings.json* přepisují hodnoty nastavené v prostředí systému.

Následující kód JSON ukazuje tři profily *launchSettings.json* souboru:

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
> `applicationUrl` Vlastnost *launchSettings.json* můžete zadat seznam adres URL serveru. Použijte středník mezi adresy URL v seznamu:
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

Při spuštění aplikace s [dotnet spustit](/dotnet/core/tools/dotnet-run), první profil s `"commandName": "Project"` se používá. Hodnota `commandName` specifikuje webový server ke spuštění. `commandName` může být jedna z následujících akcí:

* IIS Express
* IIS
* Projekt (které spustí Kestrel)

Když je aplikace spuštěna s [dotnet spustit](/dotnet/core/tools/dotnet-run):

* *launchSettings.json* je pro čtení. Pokud je k dispozici. `environmentVariables` nastavení v *launchSettings.json* přepsání proměnných prostředí.
* Hostitelské prostředí se zobrazí.

Následující výstup zobrazuje aplikaci začít [dotnet spustit](/dotnet/core/tools/dotnet-run):

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Vlastnosti projektu sady Visual Studio **ladění** karta poskytuje grafické uživatelské rozhraní pro úpravy *launchSettings.json* souboru:

![Proměnné prostředí nastavení vlastností projektu](environments/_static/project-properties-debug.png)

Změny provedené v projektu profily pravděpodobně projeví až po restartu webového serveru. Kestrel je nutné restartovat předtím, než může zjistit změny provedené v prostředí.

> [!WARNING]
> *launchSettings.json* neměli ukládat tajné kódy. [Nástroj tajný klíč správce](xref:security/app-secrets) slouží k ukládání tajných kódů pro místní vývoj.

Při použití [Visual Studio Code](https://code.visualstudio.com/), proměnné prostředí můžete nastavit v *.vscode/launch.json* souboru. Následující příklad nastaví prostředí `Development`:

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

A *.vscode/launch.json* soubor v projektu pro není čtení při spuštění aplikace s `dotnet run` stejným způsobem jako *Properties/launchSettings.json*. Zobrazit *konfigurace podle prostředí* Další informace.

### <a name="production"></a>Produkční

Na základě prostředí při spuštění třídy a metody Při spuštění aplikace ASP.NET Core třídu pro spuštění bootstraps aplikace.

* Pokud  existuje třída, pro, který se nazývá třídy :
* WebHostBuilder.UseStartupTStartup  přepíše konfigurační oddíly funkce.
* Konfigurace a ConfigureServices podpory verzí specifických pro prostředí formu  a :
* IHostingEnvironment.EnvironmentName
* Produkční protokolování a monitorování povoleno. Například [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="set-the-environment"></a>Nastavení prostředí

Často je užitečné k nastavení konkrétního prostředí pro testování. Pokud prostředí není nastavená, použije se výchozí `Production`, která zakáže většina funkcí ladění. Metoda pro nastavení prostředí závisí na operačním systému.

### <a name="azure-app-service"></a>Azure App Service

Chcete-li nastavit prostředí [služby Azure App Service](https://azure.microsoft.com/services/app-service/), proveďte následující kroky:

1. Vyberte aplikaci, ze **App Services** okno.
1. V **nastavení** skupiny, vyberte **nastavení aplikace** okno.
1. V **nastavení aplikace** vyberte **přidat nové nastavení**.
1. Pro **zadejte název**, poskytují `ASPNETCORE_ENVIRONMENT`. Pro **zadejte hodnotu**, poskytovat prostředí (například `Staging`).
1. Vyberte **nastavení slotu** zaškrtávací políčko, pokud chcete nastavení prostředí, které zůstane s aktuálním slotu, když se Prohodit sloty nasazení. Další informace najdete v tématu [dokumentaci k Azure: nastavení, které jsou přehozeny?](/azure/app-service/web-sites-staged-publishing).
1. Vyberte **Uložit** v horní části okna.

Po nastavení aplikace (proměnnou prostředí) přidání, změně nebo odstranění na webu Azure Portal, Azure App Service automaticky restartuje aplikaci.

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

Tyto příkazy platit jenom pro aktuální okno. Při zavření okna `ASPNETCORE_ENVIRONMENT` nastavení obnoví na výchozí nastavení nebo počítač hodnotu. Chcete-li nastavit hodnotu globálně ve Windows, otevřete **ovládací panely** > **systému** > **Upřesnit nastavení systému** a přidat nebo upravit `ASPNETCORE_ENVIRONMENT`hodnotu:

![Systémové rozšířené vlastnosti](environments/_static/systemsetting_environment.png)

![Proměnná prostředí základní ASPNET](environments/_static/windows_aspnetcore_environment.png)

**web.config**

Najdete v článku *nastavení proměnných prostředí* část [odkaz Konfigurace modul ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tématu.

**Na jeden fond aplikací služby IIS**

Nastavení proměnných prostředí pro jednotlivé aplikace spuštěné v izolované fondy aplikací (podporované ve službě IIS 10.0 a novější), najdete v článku *AppCmd.exe příkaz* část [proměnné prostředí &lt; environmentVariables&gt; ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tématu.

### <a name="macos"></a>macOS

Nastavení aktuální prostředí pro macOS může být prováděny v řádku při spuštění aplikace:

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

Můžete také nastavit prostředí pomocí `export` před spuštěním aplikace:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

Proměnné prostředí na úrovni počítače se nastavují *.bashrc* nebo *.bash_profile* souboru. Upravte soubor pomocí libovolného textového editoru. Přidejte následující příkaz:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux

Pro distribuce Linuxu, použijte `export` příkazu na příkazovém řádku pro nastavení proměnné na základě relace a *bash_profile* souboru pro nastavení prostředí na úrovni počítače.

### <a name="configuration-by-environment"></a>Konfigurace podle prostředí

Zobrazit [konfigurace podle prostředí](xref:fundamentals/configuration/index#configuration-by-environment) Další informace.

## <a name="environment-based-startup-class-and-methods"></a>Na základě prostředí při spuštění třídy a metody

Při spuštění aplikace ASP.NET Core [třídu pro spuštění](xref:fundamentals/startup) bootstraps aplikace. Pokud `Startup{EnvironmentName}` existuje třída, pro, který se nazývá třídy `EnvironmentName`:

[!code-csharp[](environments/sample/EnvironmentsSample/StartupDev.cs?name=snippet&highlight=1)]

[WebHostBuilder.UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) přepíše konfigurační oddíly funkce.

[Konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) podpory verzí specifických pro prostředí formu `Configure{EnvironmentName}` a `Configure{EnvironmentName}Services`:

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a>Další zdroje

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
