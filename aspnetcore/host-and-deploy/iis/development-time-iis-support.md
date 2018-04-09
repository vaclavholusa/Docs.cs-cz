---
title: Podpora IIS době vývoje v sadě Visual Studio pro ASP.NET Core
author: shirhatti
description: Zjistit, podpora ladění aplikací ASP.NET Core při spuštění za služby IIS v systému Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 218bb2653b92cd7b1cf2c6726b2d4bedbf307a62
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="96231-103">Podpora IIS době vývoje v sadě Visual Studio pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="96231-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="96231-104">Podle [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="96231-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="96231-105">Tento článek popisuje [Visual Studio](https://www.visualstudio.com/vs/) podporu pro ladění aplikací ASP.NET Core za služby IIS v systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="96231-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="96231-106">Toto téma vás provede povolení této funkce a nastavení projektu.</span><span class="sxs-lookup"><span data-stu-id="96231-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="96231-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="96231-107">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="96231-108">Povolení služby IIS</span><span class="sxs-lookup"><span data-stu-id="96231-108">Enable IIS</span></span>

<span data-ttu-id="96231-109">Povolte službu IIS.</span><span class="sxs-lookup"><span data-stu-id="96231-109">Enable IIS.</span></span> <span data-ttu-id="96231-110">Přejděte na **ovládací panely** > **programy** > **programy a funkce** > **zapnout Windows funkce na nebo vypnout** (levé straně obrazovky).</span><span class="sxs-lookup"><span data-stu-id="96231-110">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="96231-111">Vyberte **Internetová informační služba** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="96231-111">Select the **Internet Information Services** checkbox.</span></span>

![Zobrazuje zaškrtávací políčko Internetová informační služba zaškrtnutí jako černé hranaté (ne zaškrtnutí) označující, že některé funkce služby IIS jsou povoleny funkce systému Windows](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="96231-113">Pokud instalace služby IIS vyžaduje restartování, restartování systému.</span><span class="sxs-lookup"><span data-stu-id="96231-113">If the IIS installation requires a restart, restart the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="96231-114">Povolit podporu služby IIS vývoj čas</span><span class="sxs-lookup"><span data-stu-id="96231-114">Enable development-time IIS support</span></span>

<span data-ttu-id="96231-115">Spusťte instalační program sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="96231-115">Launch the Visual Studio installer.</span></span> <span data-ttu-id="96231-116">Vyberte **okamžiku vývoje služby IIS podporují** součásti.</span><span class="sxs-lookup"><span data-stu-id="96231-116">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="96231-117">Součást je uveden jako volitelný v **Souhrn** panelu pro **ASP.NET a webové vývoj** zatížení.</span><span class="sxs-lookup"><span data-stu-id="96231-117">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="96231-118">Tím se nainstaluje [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module), což je potřebné ke spuštění aplikace ASP.NET Core nativní modul služby IIS.</span><span class="sxs-lookup"><span data-stu-id="96231-118">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps.</span></span>

![Úprava funkcích nástroje Visual Studio: je zvolena karta The úlohy.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="96231-122">Konfigurace projektu</span><span class="sxs-lookup"><span data-stu-id="96231-122">Configure the project</span></span>

<span data-ttu-id="96231-123">Vytvořte nový profil spuštění přidat podporu vývoj služby IIS.</span><span class="sxs-lookup"><span data-stu-id="96231-123">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="96231-124">V sadě Visual Studio **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="96231-124">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="96231-125">Vyberte **ladění** kartě. Vyberte **IIS** z **spusťte** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="96231-125">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="96231-126">Potvrďte, že **spuštění prohlížeče** je funkce s správnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="96231-126">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Okno vlastností projektu s kartu ladění vybrané.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="96231-131">Případně ručně přidat profil spuštění, který [launchSettings.json](http://json.schemastore.org/launchsettings) souboru v aplikaci:</span><span class="sxs-lookup"><span data-stu-id="96231-131">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="96231-132">Visual Studio může výzvu restartování, pokud není spuštěna jako správce.</span><span class="sxs-lookup"><span data-stu-id="96231-132">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="96231-133">Pokud se zobrazí výzva, restartujte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="96231-133">If prompted, restart Visual Studio.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="96231-134">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="96231-134">Additional resources</span></span>

* [<span data-ttu-id="96231-135">Jádro ASP.NET hostitele v systému Windows pomocí služby IIS</span><span class="sxs-lookup"><span data-stu-id="96231-135">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="96231-136">Úvod do modulu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="96231-136">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="96231-137">Referenční dokumentace k modulu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="96231-137">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
