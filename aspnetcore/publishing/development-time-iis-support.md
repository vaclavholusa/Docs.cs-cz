---
title: "Podpora IIS době vývoje v sadě Visual Studio pro ASP.NET Core"
author: shirhatti
description: "Zjistit, podpora ladění aplikací ASP.NET Core při spuštění za služby IIS v systému Windows Server."
keywords: "ASP.NET Core, Internetové informační služby, služby iis, modulu jádra windows server,asp.net, ladění"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.assetid: 83d98477-9d10-4a78-a54a-f325ad67d13b
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/development-time-iis-support
ms.openlocfilehash: a35a6fd9896c4c110d1b6680b6aaf718d29a18ab
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="01e8c-104">Podpora IIS době vývoje v sadě Visual Studio pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="01e8c-104">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="01e8c-105">Pomocí: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="01e8c-105">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="01e8c-106">Tento článek popisuje [Visual Studio](https://www.visualstudio.com/vs/) podporu pro ladění aplikací ASP.NET Core spuštěné za služby IIS v systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="01e8c-106">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="01e8c-107">Toto téma vás provede povolení této funkce a nastavení projektu.</span><span class="sxs-lookup"><span data-stu-id="01e8c-107">This topic walks you through enabling this feature and setting up your project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01e8c-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="01e8c-108">Prerequisites</span></span>

* <span data-ttu-id="01e8c-109">Visual Studio (2017/verze 15.3 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="01e8c-109">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="01e8c-110">ASP.NET a webové úlohy vývoj *nebo* zatížení vývoj pro různé platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="01e8c-110">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="01e8c-111">Povolení služby IIS</span><span class="sxs-lookup"><span data-stu-id="01e8c-111">Enable IIS</span></span>

<span data-ttu-id="01e8c-112">Povolte službu IIS v systému.</span><span class="sxs-lookup"><span data-stu-id="01e8c-112">Enable IIS on your system.</span></span> <span data-ttu-id="01e8c-113">Přejděte na **ovládací panely** > **programy** > **programy a funkce** > **zapnout Windows funkce na nebo vypnout** (levé straně obrazovky).</span><span class="sxs-lookup"><span data-stu-id="01e8c-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="01e8c-114">Vyberte **Internetová informační služba** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="01e8c-114">Select the **Internet Information Services** checkbox.</span></span>

![Zobrazuje zaškrtávací políčko Internetová informační služba zaškrtnutí jako černé hranaté (ne zaškrtnutí) označující, že některé funkce služby IIS jsou povoleny funkce systému Windows](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="01e8c-116">Pokud instalace služby IIS vyžaduje restartování, restartování systému.</span><span class="sxs-lookup"><span data-stu-id="01e8c-116">If your IIS installation requires a reboot, reboot your system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="01e8c-117">Povolit podporu služby IIS vývoj čas</span><span class="sxs-lookup"><span data-stu-id="01e8c-117">Enable development-time IIS support</span></span>

<span data-ttu-id="01e8c-118">Po instalaci služby IIS, spusťte instalační program sady Visual Studio k úpravě existující instalace Visual Studia.</span><span class="sxs-lookup"><span data-stu-id="01e8c-118">Once you've installed IIS, launch the Visual Studio installer to modify your existing Visual Studio installation.</span></span> <span data-ttu-id="01e8c-119">V instalačním programu vyberte **okamžiku vývoje služby IIS podporují** součásti.</span><span class="sxs-lookup"><span data-stu-id="01e8c-119">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="01e8c-120">Součást je uveden jako volitelná součást systému **Souhrn** panelu pro **ASP.NET a webové vývoj** zatížení.</span><span class="sxs-lookup"><span data-stu-id="01e8c-120">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="01e8c-121">Tím se nainstaluje [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module), což je nativní modul IIS vyžaduje ke spuštění aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="01e8c-121">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Úprava funkcích nástroje Visual Studio: je zvolena karta The úlohy.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="01e8c-125">Konfigurace projektu</span><span class="sxs-lookup"><span data-stu-id="01e8c-125">Configure the project</span></span>

<span data-ttu-id="01e8c-126">Vytvořte nový profil spuštění přidat podporu vývoj služby IIS.</span><span class="sxs-lookup"><span data-stu-id="01e8c-126">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="01e8c-127">V sadě Visual Studio **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="01e8c-127">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="01e8c-128">Vyberte **ladění** kartě. Vyberte **IIS** z **spusťte** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="01e8c-128">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="01e8c-129">Potvrďte, že **spuštění prohlížeče** je funkce s správnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="01e8c-129">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Okno vlastností projektu s kartu ladění vybrané.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="01e8c-134">Alternativně můžete ručně přidat profil spuštění, který vaše [launchSettings.json](http://json.schemastore.org/launchsettings) souboru v aplikaci:</span><span class="sxs-lookup"><span data-stu-id="01e8c-134">Alternatively, you can manually add a launch profile to your [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="01e8c-135">Můžete být vyzváni k restartování sady Visual Studio, pokud nebyly spustit jako správce.</span><span class="sxs-lookup"><span data-stu-id="01e8c-135">You may be prompted to restart Visual Studio if you weren't running as an administrator.</span></span> <span data-ttu-id="01e8c-136">Pokud se zobrazí výzva, restartujte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01e8c-136">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="01e8c-137">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="01e8c-137">Congratulations!</span></span> <span data-ttu-id="01e8c-138">V tomto okamžiku projektu je nakonfigurován pro podporu vývoj čas IIS.</span><span class="sxs-lookup"><span data-stu-id="01e8c-138">At this point, your project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="01e8c-139">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="01e8c-139">Additional resources</span></span>

* [<span data-ttu-id="01e8c-140">Jádro ASP.NET hostitele v systému Windows pomocí služby IIS</span><span class="sxs-lookup"><span data-stu-id="01e8c-140">Host ASP.NET Core on Windows with IIS</span></span>](xref:publishing/iis)
* [<span data-ttu-id="01e8c-141">Úvod do modulu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="01e8c-141">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="01e8c-142">Odkaz na konfiguraci základní modul ASP.NET</span><span class="sxs-lookup"><span data-stu-id="01e8c-142">ASP.NET Core Module configuration reference</span></span>](xref:hosting/aspnet-core-module)
