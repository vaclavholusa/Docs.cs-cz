---
title: "Podpora IIS době vývoje v sadě Visual Studio pro ASP.NET Core"
author: shirhatti
description: "Zjistit, podpora ladění aplikací ASP.NET Core při spuštění za služby IIS v systému Windows Server."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 264be49e8aff72f913c22508150e424541d49fa0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Podpora IIS době vývoje v sadě Visual Studio pro ASP.NET Core

Pomocí: [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Tento článek popisuje [Visual Studio](https://www.visualstudio.com/vs/) podporu pro ladění aplikací ASP.NET Core spuštěné za služby IIS v systému Windows Server. Toto téma vás provede povolení této funkce a nastavení projektu.

## <a name="prerequisites"></a>Požadavky

* Visual Studio (2017/verze 15.3 nebo novější)
* ASP.NET a webové úlohy vývoj *nebo* zatížení vývoj pro různé platformy .NET Core

## <a name="enable-iis"></a>Povolení služby IIS

Povolte službu IIS. Přejděte na **ovládací panely** > **programy** > **programy a funkce** > **zapnout Windows funkce na nebo vypnout** (levé straně obrazovky). Vyberte **Internetová informační služba** zaškrtávací políčko.

![Zobrazuje zaškrtávací políčko Internetová informační služba zaškrtnutí jako černé hranaté (ne zaškrtnutí) označující, že některé funkce služby IIS jsou povoleny funkce systému Windows](development-time-iis-support/_static/enable_iis.png)

Pokud instalace služby IIS vyžaduje restartování, restartování systému.

## <a name="enable-development-time-iis-support"></a>Povolit podporu služby IIS vývoj čas

Po instalaci služby IIS, spusťte instalační program sady Visual Studio upravte stávající instalaci sady Visual Studio. V instalačním programu vyberte **okamžiku vývoje služby IIS podporují** součásti. Součást je uveden jako volitelná součást systému **Souhrn** panelu pro **ASP.NET a webové vývoj** zatížení. Tím se nainstaluje [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module), což je nativní modul IIS vyžaduje ke spuštění aplikací ASP.NET Core.

![Úprava funkcích nástroje Visual Studio: je zvolena karta The úlohy. V části Web a Cloud panelu vývoj ASP.NET a webové zaškrtnuto. Na pravé straně v oblasti volitelné souhrnné panelu je zaškrtávací políčko pro vývoj, když služba IIS podporovat.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Konfigurace projektu

Vytvořte nový profil spuštění přidat podporu vývoj služby IIS. V sadě Visual Studio **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **vlastnosti**. Vyberte **ladění** kartě. Vyberte **IIS** z **spusťte** rozevíracího seznamu. Potvrďte, že **spuštění prohlížeče** je funkce s správnou adresu URL.

![Okno vlastností projektu s kartu ladění vybrané. Nastavení profilu a spuštění je nastaveno do služby IIS. Funkce spuštění prohlížeče je povolená s adresu http://localhost/WebApplication2. Stejnou adresu jsou tu taky v poli Adresa URL aplikace v oblasti nastavení webového serveru s povoleno povolit anonymní ověřování.](development-time-iis-support/_static/project_properties.png)

Případně ručně přidat profil spuštění, který [launchSettings.json](http://json.schemastore.org/launchsettings) souboru v aplikaci:

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

Visual Studio může výzvu restartování, pokud není spuštěna jako správce. Pokud se zobrazí výzva, restartujte Visual Studio.

Blahopřejeme! V tomto okamžiku projekt je konfigurován pro podporu vývoj čas IIS. 

## <a name="additional-resources"></a>Další zdroje

* [Jádro ASP.NET hostitele v systému Windows pomocí služby IIS](xref:host-and-deploy/iis/index)
* [Úvod do modulu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Referenční dokumentace k modulu ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
