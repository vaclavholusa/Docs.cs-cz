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
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="4160d-103">Podpora IIS době vývoje v sadě Visual Studio pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4160d-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="4160d-104">Podle [Sourabh Shirhatti](https://twitter.com/sshirhatti) a [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4160d-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4160d-105">Tento článek popisuje [Visual Studio](https://www.visualstudio.com/vs/) podporu pro ladění aplikací ASP.NET Core za služby IIS v systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="4160d-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="4160d-106">Toto téma vás provede povolení této funkce a nastavení projektu.</span><span class="sxs-lookup"><span data-stu-id="4160d-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4160d-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4160d-107">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="4160d-108">Povolení služby IIS</span><span class="sxs-lookup"><span data-stu-id="4160d-108">Enable IIS</span></span>

1. <span data-ttu-id="4160d-109">Přejděte na **ovládací panely** > **programy** > **programy a funkce** > **zapnout Windows funkce na nebo vypnout** (levé straně obrazovky).</span><span class="sxs-lookup"><span data-stu-id="4160d-109">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="4160d-110">Vyberte **Internetová informační služba** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="4160d-110">Select the **Internet Information Services** check box.</span></span>

![Funkce systému Windows zobrazující Internetová informační služba zaškrtávací políčko zaškrtnuto jako černá čtverce (ne zaškrtnutí) označující, že některé funkce služby IIS jsou povolené](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="4160d-112">Instalace služby IIS může vyžadovat restartování systému.</span><span class="sxs-lookup"><span data-stu-id="4160d-112">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="4160d-113">Konfigurace služby IIS</span><span class="sxs-lookup"><span data-stu-id="4160d-113">Configure IIS</span></span>

<span data-ttu-id="4160d-114">Služba IIS musí mít web nakonfigurované s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="4160d-114">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="4160d-115">Název hostitele, který odpovídá název hostitele adresy URL profilu spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="4160d-115">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="4160d-116">Vazba pro port 443 přiřazené certifikátem.</span><span class="sxs-lookup"><span data-stu-id="4160d-116">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="4160d-117">Například **název hostitele** pro přidání webu je nastaven na "localhost" (profil spuštění bude také použít "localhost" dál v tomto tématu).</span><span class="sxs-lookup"><span data-stu-id="4160d-117">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="4160d-118">Je port nastaven na hodnotu "443" (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="4160d-118">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="4160d-119">**Služby IIS Express vývojový certifikát** je přiřazen k webu, ale libovolný platný certifikát funguje:</span><span class="sxs-lookup"><span data-stu-id="4160d-119">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Přidejte okno webu ve službě IIS zobrazující sadu vazby pro místního hostitele na portu 443 s certifikátem přiřazen.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="4160d-121">Pokud má již instalace služby IIS **Default Web Site** s názvem hostitele, který odpovídá název hostitele adresy URL profilu spuštění aplikace:</span><span class="sxs-lookup"><span data-stu-id="4160d-121">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="4160d-122">Přidáte vazbu port pro port 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="4160d-122">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="4160d-123">Platný certifikát přiřadíte webu.</span><span class="sxs-lookup"><span data-stu-id="4160d-123">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="4160d-124">Povolit podporu služby IIS době vývoje v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4160d-124">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="4160d-125">Spusťte instalační program sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4160d-125">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="4160d-126">Vyberte **okamžiku vývoje služby IIS podporují** součásti.</span><span class="sxs-lookup"><span data-stu-id="4160d-126">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="4160d-127">Součást je uveden jako volitelný v **Souhrn** panelu pro **ASP.NET a webové vývoj** zatížení.</span><span class="sxs-lookup"><span data-stu-id="4160d-127">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="4160d-128">Součást nainstaluje [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module), což je nutná k provozování ASP.NET Core aplikace za služby IIS v konfiguraci reverzní proxy server nativní modul služby IIS.</span><span class="sxs-lookup"><span data-stu-id="4160d-128">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![Úprava funkcích nástroje Visual Studio: je zvolena karta The úlohy.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="4160d-132">Konfigurace projektu</span><span class="sxs-lookup"><span data-stu-id="4160d-132">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="4160d-133">Přesměrování protokolu HTTPS</span><span class="sxs-lookup"><span data-stu-id="4160d-133">HTTPS redirection</span></span>

<span data-ttu-id="4160d-134">Pro nový projekt, zaškrtněte políčko pro **konfigurace pro protokol HTTPS** v **nové webové aplikace ASP.NET Core** okno:</span><span class="sxs-lookup"><span data-stu-id="4160d-134">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Nové webové aplikace ASP.NET Core okno s konfigurací pro HTTPS zaškrtnutým políčkem.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="4160d-136">V existujícího projektu, použijte protokol HTTPS přesměrování Middleware v `Startup.Configure` voláním [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="4160d-136">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

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

### <a name="iis-launch-profile"></a><span data-ttu-id="4160d-137">Služba IIS spuštění profilu</span><span class="sxs-lookup"><span data-stu-id="4160d-137">IIS launch profile</span></span>

<span data-ttu-id="4160d-138">Vytvořte nový profil spuštění pro přidání podpory služby IIS vývoj čas:</span><span class="sxs-lookup"><span data-stu-id="4160d-138">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="4160d-139">Pro **profil**, vyberte **nový** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4160d-139">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="4160d-140">Název profilu "IIS" v místním okně.</span><span class="sxs-lookup"><span data-stu-id="4160d-140">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="4160d-141">Vyberte **OK** chcete vytvořit profil.</span><span class="sxs-lookup"><span data-stu-id="4160d-141">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="4160d-142">Pro **spusťte** nastavení, vyberte **IIS** ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="4160d-142">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="4160d-143">Zaškrtněte políčko pro **spuštění prohlížeče** a zadejte adresu URL koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="4160d-143">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="4160d-144">Použijte protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4160d-144">Use the HTTPS protocol.</span></span> <span data-ttu-id="4160d-145">Tento příklad používá `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="4160d-145">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="4160d-146">V **proměnné prostředí** vyberte **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4160d-146">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="4160d-147">Zadejte proměnnou prostředí s klíčem pro `ASPNETCORE_ENVIRONMENT` a hodnota `Development`.</span><span class="sxs-lookup"><span data-stu-id="4160d-147">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="4160d-148">V **nastavení webového serveru** oblasti, nastavte **adresu URL aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4160d-148">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="4160d-149">Tento příklad používá `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="4160d-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="4160d-150">Uložení profilu.</span><span class="sxs-lookup"><span data-stu-id="4160d-150">Save the profile.</span></span>

![Okno vlastností projektu s kartu ladění vybrané.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="4160d-155">Případně ručně přidat profil spuštění, který [launchSettings.json](http://json.schemastore.org/launchsettings) souboru v aplikaci:</span><span class="sxs-lookup"><span data-stu-id="4160d-155">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="4160d-156">Spusťte projekt</span><span class="sxs-lookup"><span data-stu-id="4160d-156">Run the project</span></span>

<span data-ttu-id="4160d-157">V uživatelském rozhraní VS nastavit na tlačítko Spustit **IIS** profilu a kliknutím na tlačítko spustit aplikaci:</span><span class="sxs-lookup"><span data-stu-id="4160d-157">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

![Tlačítko spusťte na panelu nástrojů VS nastavení pro profily "IIS".](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="4160d-159">Visual Studio může výzvu restartování, pokud není spuštěna jako správce.</span><span class="sxs-lookup"><span data-stu-id="4160d-159">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="4160d-160">Pokud se zobrazí výzva, restartujte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4160d-160">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="4160d-161">Pokud se používá certifikát nedůvěryhodné vývoj, může vyžadovat prohlížeče vám umožní vytvořit výjimku pro nedůvěryhodný certifikát.</span><span class="sxs-lookup"><span data-stu-id="4160d-161">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4160d-162">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4160d-162">Additional resources</span></span>

* [<span data-ttu-id="4160d-163">Jádro ASP.NET hostitele v systému Windows pomocí služby IIS</span><span class="sxs-lookup"><span data-stu-id="4160d-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="4160d-164">Úvod do modulu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4160d-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="4160d-165">Referenční dokumentace k modulu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4160d-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="4160d-166">Vynucení protokolu HTTPS</span><span class="sxs-lookup"><span data-stu-id="4160d-166">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
