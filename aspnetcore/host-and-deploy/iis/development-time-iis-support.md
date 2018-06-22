---
title: Podpora IIS době vývoje v sadě Visual Studio pro ASP.NET Core
author: shirhatti
description: Zjistit, podpora ladění aplikací ASP.NET Core při spuštění za služby IIS v systému Windows Server.
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: eb8b4369d6d5434adbac187f59b18d7a2b80055c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277651"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="15889-103">Podpora IIS době vývoje v sadě Visual Studio pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15889-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="15889-104">Podle [Sourabh Shirhatti](https://twitter.com/sshirhatti) a [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="15889-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="15889-105">Tento článek popisuje [Visual Studio](https://www.visualstudio.com/vs/) podporu pro ladění aplikací ASP.NET Core za služby IIS v systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="15889-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="15889-106">Toto téma vás provede povolení této funkce a nastavení projektu.</span><span class="sxs-lookup"><span data-stu-id="15889-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15889-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="15889-107">Prerequisites</span></span>

* [<span data-ttu-id="15889-108">Visual Studio pro Windows</span><span class="sxs-lookup"><span data-stu-id="15889-108">Visual Studio for Windows</span></span>](https://www.microsoft.com/net/download/windows)
* <span data-ttu-id="15889-109">**Vývoj pro ASP.NET a webové** pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="15889-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="15889-110">**Vývoj pro různé platformy .NET core** pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="15889-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="15889-111">Certifikát X.509 zabezpečení</span><span class="sxs-lookup"><span data-stu-id="15889-111">X.509 security certificate</span></span>

## <a name="enable-iis"></a><span data-ttu-id="15889-112">Povolení služby IIS</span><span class="sxs-lookup"><span data-stu-id="15889-112">Enable IIS</span></span>

1. <span data-ttu-id="15889-113">Přejděte na **ovládací panely** > **programy** > **programy a funkce** > **zapnout Windows funkce na nebo vypnout** (levé straně obrazovky).</span><span class="sxs-lookup"><span data-stu-id="15889-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="15889-114">Vyberte **Internetová informační služba** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="15889-114">Select the **Internet Information Services** check box.</span></span>

![Funkce systému Windows zobrazující Internetová informační služba zaškrtávací políčko zaškrtnuto jako černá čtverce (ne zaškrtnutí) označující, že některé funkce služby IIS jsou povolené](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="15889-116">Instalace služby IIS může vyžadovat restartování systému.</span><span class="sxs-lookup"><span data-stu-id="15889-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="15889-117">Konfigurace služby IIS</span><span class="sxs-lookup"><span data-stu-id="15889-117">Configure IIS</span></span>

<span data-ttu-id="15889-118">Služba IIS musí mít web nakonfigurované s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="15889-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="15889-119">Název hostitele, který odpovídá název hostitele adresy URL profilu spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="15889-119">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="15889-120">Vazba pro port 443 přiřazené certifikátem.</span><span class="sxs-lookup"><span data-stu-id="15889-120">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="15889-121">Například **název hostitele** pro přidání webu je nastaven na "localhost" (profil spuštění bude také použít "localhost" dál v tomto tématu).</span><span class="sxs-lookup"><span data-stu-id="15889-121">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="15889-122">Je port nastaven na hodnotu "443" (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="15889-122">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="15889-123">**Služby IIS Express vývojový certifikát** je přiřazen k webu, ale libovolný platný certifikát funguje:</span><span class="sxs-lookup"><span data-stu-id="15889-123">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Přidejte okno webu ve službě IIS zobrazující sadu vazby pro místního hostitele na portu 443 s certifikátem přiřazen.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="15889-125">Pokud má již instalace služby IIS **Default Web Site** s názvem hostitele, který odpovídá název hostitele adresy URL profilu spuštění aplikace:</span><span class="sxs-lookup"><span data-stu-id="15889-125">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="15889-126">Přidáte vazbu port pro port 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="15889-126">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="15889-127">Platný certifikát přiřadíte webu.</span><span class="sxs-lookup"><span data-stu-id="15889-127">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="15889-128">Povolit podporu služby IIS době vývoje v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15889-128">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="15889-129">Spusťte instalační program sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15889-129">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="15889-130">Vyberte **okamžiku vývoje služby IIS podporují** součásti.</span><span class="sxs-lookup"><span data-stu-id="15889-130">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="15889-131">Součást je uveden jako volitelný v **Souhrn** panelu pro **ASP.NET a webové vývoj** zatížení.</span><span class="sxs-lookup"><span data-stu-id="15889-131">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="15889-132">Součást nainstaluje [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module), což je nutná k provozování ASP.NET Core aplikace za služby IIS v konfiguraci reverzní proxy server nativní modul služby IIS.</span><span class="sxs-lookup"><span data-stu-id="15889-132">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![Úprava funkcích nástroje Visual Studio: je zvolena karta The úlohy.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="15889-136">Konfigurace projektu</span><span class="sxs-lookup"><span data-stu-id="15889-136">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="15889-137">Přesměrování protokolu HTTPS</span><span class="sxs-lookup"><span data-stu-id="15889-137">HTTPS redirection</span></span>

<span data-ttu-id="15889-138">Pro nový projekt, zaškrtněte políčko pro **konfigurace pro protokol HTTPS** v **nové webové aplikace ASP.NET Core** okno:</span><span class="sxs-lookup"><span data-stu-id="15889-138">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Nové webové aplikace ASP.NET Core okno s konfigurací pro HTTPS zaškrtnutým políčkem.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="15889-140">V existujícího projektu, použijte protokol HTTPS přesměrování Middleware v `Startup.Configure` voláním [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="15889-140">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

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

### <a name="iis-launch-profile"></a><span data-ttu-id="15889-141">Služba IIS spuštění profilu</span><span class="sxs-lookup"><span data-stu-id="15889-141">IIS launch profile</span></span>

<span data-ttu-id="15889-142">Vytvořte nový profil spuštění pro přidání podpory služby IIS vývoj čas:</span><span class="sxs-lookup"><span data-stu-id="15889-142">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="15889-143">Pro **profil**, vyberte **nový** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="15889-143">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="15889-144">Název profilu "IIS" v místním okně.</span><span class="sxs-lookup"><span data-stu-id="15889-144">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="15889-145">Vyberte **OK** chcete vytvořit profil.</span><span class="sxs-lookup"><span data-stu-id="15889-145">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="15889-146">Pro **spusťte** nastavení, vyberte **IIS** ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="15889-146">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="15889-147">Zaškrtněte políčko pro **spuštění prohlížeče** a zadejte adresu URL koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="15889-147">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="15889-148">Použijte protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="15889-148">Use the HTTPS protocol.</span></span> <span data-ttu-id="15889-149">Tento příklad používá `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="15889-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="15889-150">V **proměnné prostředí** vyberte **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="15889-150">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="15889-151">Zadejte proměnnou prostředí s klíčem pro `ASPNETCORE_ENVIRONMENT` a hodnota `Development`.</span><span class="sxs-lookup"><span data-stu-id="15889-151">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="15889-152">V **nastavení webového serveru** oblasti, nastavte **adresu URL aplikace**.</span><span class="sxs-lookup"><span data-stu-id="15889-152">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="15889-153">Tento příklad používá `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="15889-153">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="15889-154">Uložení profilu.</span><span class="sxs-lookup"><span data-stu-id="15889-154">Save the profile.</span></span>

![Okno vlastností projektu s kartu ladění vybrané.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="15889-159">Případně ručně přidat profil spuštění, který [launchSettings.json](http://json.schemastore.org/launchsettings) souboru v aplikaci:</span><span class="sxs-lookup"><span data-stu-id="15889-159">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="15889-160">Spusťte projekt</span><span class="sxs-lookup"><span data-stu-id="15889-160">Run the project</span></span>

<span data-ttu-id="15889-161">V uživatelském rozhraní VS nastavit na tlačítko Spustit **IIS** profilu a kliknutím na tlačítko spustit aplikaci:</span><span class="sxs-lookup"><span data-stu-id="15889-161">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

![Tlačítko spusťte na panelu nástrojů VS nastavení pro profily "IIS".](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="15889-163">Visual Studio může výzvu restartování, pokud není spuštěna jako správce.</span><span class="sxs-lookup"><span data-stu-id="15889-163">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="15889-164">Pokud se zobrazí výzva, restartujte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15889-164">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="15889-165">Pokud se používá certifikát nedůvěryhodné vývoj, může vyžadovat prohlížeče vám umožní vytvořit výjimku pro nedůvěryhodný certifikát.</span><span class="sxs-lookup"><span data-stu-id="15889-165">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="15889-166">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="15889-166">Additional resources</span></span>

* [<span data-ttu-id="15889-167">Jádro ASP.NET hostitele v systému Windows pomocí služby IIS</span><span class="sxs-lookup"><span data-stu-id="15889-167">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="15889-168">Úvod do modulu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15889-168">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="15889-169">Referenční dokumentace k modulu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15889-169">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="15889-170">Vynucení protokolu HTTPS</span><span class="sxs-lookup"><span data-stu-id="15889-170">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
