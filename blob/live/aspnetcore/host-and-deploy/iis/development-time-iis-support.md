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
ms.openlocfilehash: 95bc6c124f4d017a4371017b3dd04fd1ca69f96d
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="bca38-103">Podpora IIS době vývoje v sadě Visual Studio pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bca38-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="bca38-104">Pomocí: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="bca38-104">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="bca38-105">Tento článek popisuje [Visual Studio](https://www.visualstudio.com/vs/) podporu pro ladění aplikací ASP.NET Core spuštěné za služby IIS v systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="bca38-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="bca38-106">Toto téma vás provede povolení této funkce a nastavení projektu.</span><span class="sxs-lookup"><span data-stu-id="bca38-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bca38-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bca38-107">Prerequisites</span></span>

* <span data-ttu-id="bca38-108">Visual Studio (2017/verze 15.3 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="bca38-108">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="bca38-109">ASP.NET a webové úlohy vývoj *nebo* zatížení vývoj pro různé platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="bca38-109">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="bca38-110">Povolení služby IIS</span><span class="sxs-lookup"><span data-stu-id="bca38-110">Enable IIS</span></span>

<span data-ttu-id="bca38-111">Povolte službu IIS.</span><span class="sxs-lookup"><span data-stu-id="bca38-111">Enable IIS.</span></span> <span data-ttu-id="bca38-112">Přejděte na **ovládací panely** > **programy** > **programy a funkce** > **zapnout Windows funkce na nebo vypnout** (levé straně obrazovky).</span><span class="sxs-lookup"><span data-stu-id="bca38-112">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="bca38-113">Vyberte **Internetová informační služba** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="bca38-113">Select the **Internet Information Services** checkbox.</span></span>

![Zobrazuje zaškrtávací políčko Internetová informační služba zaškrtnutí jako černé hranaté (ne zaškrtnutí) označující, že některé funkce služby IIS jsou povoleny funkce systému Windows](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="bca38-115">Pokud instalace služby IIS vyžaduje restartování, restartování systému.</span><span class="sxs-lookup"><span data-stu-id="bca38-115">If the IIS installation requires a reboot, reboot the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="bca38-116">Povolit podporu služby IIS vývoj čas</span><span class="sxs-lookup"><span data-stu-id="bca38-116">Enable development-time IIS support</span></span>

<span data-ttu-id="bca38-117">Po instalaci služby IIS, spusťte instalační program sady Visual Studio upravte stávající instalaci sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bca38-117">Once IIS is installed, launch the Visual Studio installer to modify the existing Visual Studio installation.</span></span> <span data-ttu-id="bca38-118">V instalačním programu vyberte **okamžiku vývoje služby IIS podporují** součásti.</span><span class="sxs-lookup"><span data-stu-id="bca38-118">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="bca38-119">Součást je uveden jako volitelná součást systému **Souhrn** panelu pro **ASP.NET a webové vývoj** zatížení.</span><span class="sxs-lookup"><span data-stu-id="bca38-119">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="bca38-120">Tím se nainstaluje [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module), což je nativní modul IIS vyžaduje ke spuštění aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bca38-120">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Úprava funkcích nástroje Visual Studio: je zvolena karta The úlohy.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="bca38-124">Konfigurace projektu</span><span class="sxs-lookup"><span data-stu-id="bca38-124">Configure the project</span></span>

<span data-ttu-id="bca38-125">Vytvořte nový profil spuštění přidat podporu vývoj služby IIS.</span><span class="sxs-lookup"><span data-stu-id="bca38-125">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="bca38-126">V sadě Visual Studio **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="bca38-126">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="bca38-127">Vyberte **ladění** kartě. Vyberte **IIS** z **spusťte** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="bca38-127">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="bca38-128">Potvrďte, že **spuštění prohlížeče** je funkce s správnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="bca38-128">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Okno vlastností projektu s kartu ladění vybrané.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="bca38-133">Případně ručně přidat profil spuštění, který [launchSettings.json](http://json.schemastore.org/launchsettings) souboru v aplikaci:</span><span class="sxs-lookup"><span data-stu-id="bca38-133">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="bca38-134">Visual Studio může výzvu restartování, pokud není spuštěna jako správce.</span><span class="sxs-lookup"><span data-stu-id="bca38-134">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="bca38-135">Pokud se zobrazí výzva, restartujte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bca38-135">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="bca38-136">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="bca38-136">Congratulations!</span></span> <span data-ttu-id="bca38-137">V tomto okamžiku projekt je konfigurován pro podporu vývoj čas IIS.</span><span class="sxs-lookup"><span data-stu-id="bca38-137">At this point, the project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="bca38-138">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="bca38-138">Additional resources</span></span>

* [<span data-ttu-id="bca38-139">Jádro ASP.NET hostitele v systému Windows pomocí služby IIS</span><span class="sxs-lookup"><span data-stu-id="bca38-139">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="bca38-140">Úvod do modulu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bca38-140">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="bca38-141">Odkaz na konfiguraci základní modul ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bca38-141">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
