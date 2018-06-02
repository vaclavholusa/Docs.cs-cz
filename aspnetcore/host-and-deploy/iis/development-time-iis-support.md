---
title: Podpora IIS době vývoje v sadě Visual Studio pro ASP.NET Core
author: shirhatti
description: Zjistit, podpora ladění aplikací ASP.NET Core při spuštění za služby IIS v systému Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: aeff8cd7da0637290d4edffaf183fc3c4f56f7f4
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/02/2018
ms.locfileid: "34555479"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Podpora IIS době vývoje v sadě Visual Studio pro ASP.NET Core

Podle [Sourabh Shirhatti](https://twitter.com/sshirhatti) a [Luke Latham](https://github.com/guardrex)

Tento článek popisuje [Visual Studio](https://www.visualstudio.com/vs/) podporu pro ladění aplikací ASP.NET Core za služby IIS v systému Windows Server. Toto téma vás provede povolení této funkce a nastavení projektu.

## <a name="prerequisites"></a>Požadavky

* [Visual Studio pro Windows](https://www.microsoft.com/net/download/windows)
* **Vývoj pro ASP.NET a webové** pracovního vytížení
* **Vývoj pro různé platformy .NET core** pracovního vytížení
* Certifikát X.509 zabezpečení

## <a name="enable-iis"></a>Povolení služby IIS

1. Přejděte na **ovládací panely** > **programy** > **programy a funkce** > **zapnout Windows funkce na nebo vypnout** (levé straně obrazovky).
1. Vyberte **Internetová informační služba** zaškrtávací políčko.

![Funkce systému Windows zobrazující Internetová informační služba zaškrtávací políčko zaškrtnuto jako černá čtverce (ne zaškrtnutí) označující, že některé funkce služby IIS jsou povolené](development-time-iis-support/_static/enable_iis.png)

Instalace služby IIS může vyžadovat restartování systému.

## <a name="configure-iis"></a>Konfigurace služby IIS

Služba IIS musí mít web nakonfigurované s následujícími službami:

* Název hostitele, který odpovídá název hostitele adresy URL profilu spuštění aplikace.
* Vazba pro port 443 přiřazené certifikátem.

Například **název hostitele** pro přidání webu je nastaven na "localhost" (profil spuštění bude také použít "localhost" dál v tomto tématu). Je port nastaven na hodnotu "443" (HTTPS). **Služby IIS Express vývojový certifikát** je přiřazen k webu, ale libovolný platný certifikát funguje:

![Přidejte okno webu ve službě IIS zobrazující sadu vazby pro místního hostitele na portu 443 s certifikátem přiřazen.](development-time-iis-support/_static/add-website-window.png)

Pokud má již instalace služby IIS **Default Web Site** s názvem hostitele, který odpovídá název hostitele adresy URL profilu spuštění aplikace:

* Přidáte vazbu port pro port 443 (HTTPS).
* Platný certifikát přiřadíte webu.

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Povolit podporu služby IIS době vývoje v sadě Visual Studio

1. Spusťte instalační program sady Visual Studio.
1. Vyberte **okamžiku vývoje služby IIS podporují** součásti. Součást je uveden jako volitelný v **Souhrn** panelu pro **ASP.NET a webové vývoj** zatížení. Součást nainstaluje [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module), což je nutná k provozování ASP.NET Core aplikace za služby IIS v konfiguraci reverzní proxy server nativní modul služby IIS.

![Úprava funkcích nástroje Visual Studio: je zvolena karta The úlohy. V části Web a Cloud panelu vývoj ASP.NET a webové zaškrtnuto. Na pravé straně v oblasti volitelné souhrnné panelu je zaškrtávací políčko dobu vývoj, které podporují službu IIS.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Konfigurace projektu

### <a name="https-redirection"></a>Přesměrování protokolu HTTPS

Pro nový projekt, zaškrtněte políčko pro **konfigurace pro protokol HTTPS** v **nové webové aplikace ASP.NET Core** okno:

![Nové webové aplikace ASP.NET Core okno s konfigurací pro HTTPS zaškrtnutým políčkem.](development-time-iis-support/_static/new-app.png)

V existujícího projektu, použijte protokol HTTPS přesměrování Middleware v `Startup.Configure` voláním [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) metoda rozšíření:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a>Služba IIS spuštění profilu

Vytvořte nový profil spuštění pro přidání podpory služby IIS vývoj čas:

1. Pro **profil**, vyberte **nový** tlačítko. Název profilu "IIS" v místním okně. Vyberte **OK** chcete vytvořit profil.
1. Pro **spusťte** nastavení, vyberte **IIS** ze seznamu.
1. Zaškrtněte políčko pro **spuštění prohlížeče** a zadejte adresu URL koncového bodu. Použijte protokol HTTPS. Tento příklad používá `https://localhost/WebApplication1`.
1. V **proměnné prostředí** vyberte **přidat** tlačítko. Zadejte proměnnou prostředí s klíčem pro `ASPNETCORE_ENVIRONMENT` a hodnota `Development`.
1. V **nastavení webového serveru** oblasti, nastavte **adresu URL aplikace**. Tento příklad používá `https://localhost/WebApplication1`.
1. Uložení profilu.

![Okno vlastností projektu s kartu ladění vybrané. Nastavení profilu a spuštění je nastaveno do služby IIS. Povolena funkce spuštění prohlížeče s adresou https://localhost/WebApplication1. Stejnou adresu jsou tu taky v poli Adresa URL aplikace v oblasti nastavení webového serveru.](development-time-iis-support/_static/project_properties.png)

Případně ručně přidat profil spuštění, který [launchSettings.json](http://json.schemastore.org/launchsettings) souboru v aplikaci:

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## <a name="run-the-project"></a>Spusťte projekt

V uživatelském rozhraní VS nastavit na tlačítko Spustit **IIS** profilu a kliknutím na tlačítko spustit aplikaci:

![Tlačítko spusťte na panelu nástrojů VS nastavení pro profily "IIS".](development-time-iis-support/_static/toolbar.png)

Visual Studio může výzvu restartování, pokud není spuštěna jako správce. Pokud se zobrazí výzva, restartujte Visual Studio.

Pokud se používá certifikát nedůvěryhodné vývoj, může vyžadovat prohlížeče vám umožní vytvořit výjimku pro nedůvěryhodný certifikát.

## <a name="additional-resources"></a>Další zdroje

* [Jádro ASP.NET hostitele v systému Windows pomocí služby IIS](xref:host-and-deploy/iis/index)
* [Úvod do modulu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Referenční dokumentace k modulu ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Vynucení protokolu HTTPS](xref:security/enforcing-ssl)
