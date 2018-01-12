---
title: "Jádro ASP.NET hostitele v systému Windows pomocí služby IIS"
author: guardrex
description: "Zjistěte, jak hostovat aplikace ASP.NET Core na Windows serveru Internetové informační služby (IIS)."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/index
ms.openlocfilehash: 01cedb4e3abb35670d2908fe8cb4367c3fd58b33
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="2c8d3-103">Jádro ASP.NET hostitele v systému Windows pomocí služby IIS</span><span class="sxs-lookup"><span data-stu-id="2c8d3-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="2c8d3-104">Podle [Luke Latham](https://github.com/guardrex) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2c8d3-104">By [Luke Latham](https://github.com/guardrex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="2c8d3-105">Podporované operační systémy</span><span class="sxs-lookup"><span data-stu-id="2c8d3-105">Supported operating systems</span></span>

<span data-ttu-id="2c8d3-106">Podporovány jsou následující operační systémy:</span><span class="sxs-lookup"><span data-stu-id="2c8d3-106">The following operating systems are supported:</span></span>

* <span data-ttu-id="2c8d3-107">Windows 7 a novější</span><span class="sxs-lookup"><span data-stu-id="2c8d3-107">Windows 7 and newer</span></span>
* <span data-ttu-id="2c8d3-108">Windows Server 2008 R2 a novějších &#8224;</span><span class="sxs-lookup"><span data-stu-id="2c8d3-108">Windows Server 2008 R2 and newer&#8224;</span></span>

<span data-ttu-id="2c8d3-109">&#8224; Koncepčně, konfigurace služby IIS, které jsou popsané v tomto dokumentu platí také pro hostování aplikací ASP.NET Core na Nano Server IIS, ale odkazovat na [ASP.NET Core pomocí služby IIS na serveru Nano](xref:tutorials/nano-server) konkrétní pokyny.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-109">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core apps on Nano Server IIS, but refer to [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) for specific instructions.</span></span>

<span data-ttu-id="2c8d3-110">[Ovladač HTTP.sys serveru](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener)) nebude fungovat v konfiguraci reverzní proxy server se službou IIS.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) won't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="2c8d3-111">Použití [Kestrel server](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="2c8d3-112">Konfigurace služby IIS</span><span class="sxs-lookup"><span data-stu-id="2c8d3-112">IIS configuration</span></span>

<span data-ttu-id="2c8d3-113">Povolit **webového serveru (IIS)** role a vytvořit služby rolí.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-113">Enable the **Web Server (IIS)** role and establish role services.</span></span>

### <a name="windows-desktop-operating-systems"></a><span data-ttu-id="2c8d3-114">Operační systémy Windows</span><span class="sxs-lookup"><span data-stu-id="2c8d3-114">Windows desktop operating systems</span></span>

<span data-ttu-id="2c8d3-115">Přejděte na **ovládací panely** > **programy** > **programy a funkce** > **zapnout Windows funkce na nebo vypnout** (levé straně obrazovky).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-115">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="2c8d3-116">Otevřete skupinu, pro **Internetová informační služba** a **nástroje webové správy**.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-116">Open the group for **Internet Information Services** and **Web Management Tools**.</span></span> <span data-ttu-id="2c8d3-117">Zaškrtněte políčko pro **konzoly pro správu služby IIS**.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-117">Check the box for **IIS Management Console**.</span></span> <span data-ttu-id="2c8d3-118">Zaškrtněte políčko pro **webové služby**.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-118">Check the box for **World Wide Web Services**.</span></span> <span data-ttu-id="2c8d3-119">Přijměte výchozí funkce pro **webové služby** nebo přizpůsobit funkce služby IIS.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-119">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

![Konzola pro správu služby IIS a webové služby jsou vybrány v funkce systému Windows.](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a><span data-ttu-id="2c8d3-121">Operační systémy Windows Server</span><span class="sxs-lookup"><span data-stu-id="2c8d3-121">Windows Server operating systems</span></span>

<span data-ttu-id="2c8d3-122">Serverové operační systémy, používat **přidat role a funkce** Průvodce prostřednictvím **spravovat** nabídky nebo na odkaz v **správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-122">For server operating systems, use the **Add Roles and Features** wizard via the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="2c8d3-123">Na **role serveru** kroku, zaškrtněte políčko pro **webového serveru (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-123">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

![Role webového serveru IIS je vybrali v kroku rolí vyberte server.](index/_static/server-roles-ws2016.png)

<span data-ttu-id="2c8d3-125">Na **služby rolí** krok, vyberte požadovaných služeb role služby IIS nebo přijměte výchozí nastavení role služby poskytované.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-125">On the **Role services** step, select the IIS role services desired or accept the default role services provided.</span></span>

![Výchozí služby rolí jsou vybrané v kroku vyberte role služeb.](index/_static/role-services-ws2016.png)

<span data-ttu-id="2c8d3-127">Pokračujte **potvrzení** krok instalace role Webový server a služby.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-127">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="2c8d3-128">Restartování serveru nebo IIS není povinné po instalaci role webového serveru (IIS).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-128">A server/IIS restart isn't required after installing the Web Server (IIS) role.</span></span>

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="2c8d3-129">Instalaci sady hostování v rozhraní .NET Core systému Windows Server</span><span class="sxs-lookup"><span data-stu-id="2c8d3-129">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="2c8d3-130">Nainstalujte [.NET jádra Windows serveru, který hostuje sady](https://aka.ms/dotnetcore-2-windowshosting) v hostitelském systému.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-130">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore-2-windowshosting) on the hosting system.</span></span> <span data-ttu-id="2c8d3-131">Sada nainstaluje rozhraní .NET Core Runtime, základní knihovny .NET a [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-131">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="2c8d3-132">Modul vytvoří reverzní proxy server mezi službou IIS a Kestrel server.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-132">The module creates the reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="2c8d3-133">Pokud systém nemá připojení k Internetu, získejte a nainstalujte [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) před instalací sady hostování v rozhraní .NET Core systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-133">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Windows Server Hosting bundle.</span></span>

2. <span data-ttu-id="2c8d3-134">Restartování systému nebo spuštění **net stop byl /y** následuje **net start w3svc** z příkazového řádku a pokračovat tam ke změně systému CESTU.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-134">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt to pick up a change to the system PATH.</span></span>

> [!NOTE]
> <span data-ttu-id="2c8d3-135">Informace o sdílené konfiguraci IIS najdete v tématu [ASP.NET Core modul s sdílenou konfiguraci IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-135">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="2c8d3-136">Nainstaluje Web Deploy při publikování pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c8d3-136">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="2c8d3-137">Při nasazování aplikací na servery s [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), nainstalujte nejnovější verzi nástroje nasazení webu na serveru.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-137">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="2c8d3-138">Instalovat nasazení webu, použijte [instalačního programu webové platformy (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) nebo získat instalační program přímo z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-138">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="2c8d3-139">Upřednostňuje se má používat službu WebPI.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-139">The preferred method is to use WebPI.</span></span> <span data-ttu-id="2c8d3-140">WebPI nabízí samostatná instalace a konfigurace pro poskytovatele hostingových služeb.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-140">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="2c8d3-141">Konfigurace aplikace</span><span class="sxs-lookup"><span data-stu-id="2c8d3-141">Application configuration</span></span>

### <a name="enabling-the-iisintegration-components"></a><span data-ttu-id="2c8d3-142">Povolení IISIntegration součástí</span><span class="sxs-lookup"><span data-stu-id="2c8d3-142">Enabling the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2c8d3-143">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="2c8d3-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2c8d3-144">Typické *Program.cs* volání [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zahájíte nastavení hostitele.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-144">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="2c8d3-145">`CreateDefaultBuilder`nakonfiguruje [Kestrel](xref:fundamentals/servers/kestrel) jako integrace webového serveru a umožňuje služby IIS tak, že nakonfigurujete základní cesta a port pro [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module):</span><span class="sxs-lookup"><span data-stu-id="2c8d3-145">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2c8d3-146">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="2c8d3-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2c8d3-147">Zahrnout závislost na [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) balíček v závislosti aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-147">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="2c8d3-148">Integrace služby IIS middleware začlenit do aplikace tak, že přidáte *UseIISIntegration* metody rozšíření pro *WebHostBuilder*:</span><span class="sxs-lookup"><span data-stu-id="2c8d3-148">Incorporate IIS Integration middleware into the app by adding the *UseIISIntegration* extension method to *WebHostBuilder*:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="2c8d3-149">Obě `UseKestrel` a `UseIISIntegration` jsou povinné.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-149">Both `UseKestrel` and `UseIISIntegration` are required.</span></span> <span data-ttu-id="2c8d3-150">Volání kódu `UseIISIntegration` nemá vliv přenositelnost kódu.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-150">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="2c8d3-151">Pokud aplikace není spuštěna za služby IIS (například spuštění aplikace přímo na Kestrel), `UseIISIntegration` ops ne.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-151">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` no-ops.</span></span>

---

<span data-ttu-id="2c8d3-152">Další informace o hostování najdete v tématu [hostování v ASP.NET Core](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-152">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="2c8d3-153">Možnosti služby IIS</span><span class="sxs-lookup"><span data-stu-id="2c8d3-153">IIS options</span></span>

<span data-ttu-id="2c8d3-154">Chcete-li nakonfigurovat možnosti služby IIS, obsahovat konfigurace služby pro `IISOptions` v `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2c8d3-154">To configure IIS options, include a service configuration for `IISOptions` in `ConfigureServices`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| <span data-ttu-id="2c8d3-155">Možnost</span><span class="sxs-lookup"><span data-stu-id="2c8d3-155">Option</span></span>                         | <span data-ttu-id="2c8d3-156">Výchozí</span><span class="sxs-lookup"><span data-stu-id="2c8d3-156">Default</span></span> | <span data-ttu-id="2c8d3-157">Nastavení</span><span class="sxs-lookup"><span data-stu-id="2c8d3-157">Setting</span></span> |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="2c8d3-158">Pokud `true`, nastaví middleware ověřování `HttpContext.User` a reaguje na obecné problémy.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-158">If `true`, the authentication middleware sets the `HttpContext.User` and responds to generic challenges.</span></span> <span data-ttu-id="2c8d3-159">Pokud `false`, middleware ověřování se poskytuje pouze identity (`HttpContext.User`) a reaguje na výzvy explicitně žádost `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-159">If `false`, the authentication middleware only provides an identity (`HttpContext.User`) and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="2c8d3-160">Ověřování systému Windows musí být povolené ve službě IIS pro `AutomaticAuthentication` funkce.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-160">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="2c8d3-161">Nastaví zobrazovaný název zobrazovat uživatelům na přihlašovací stránky.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-161">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="2c8d3-162">Pokud `true` a `MS-ASPNETCORE-CLIENTCERT` nachází hlavička požadavku `HttpContext.Connection.ClientCertificate` se naplní.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-162">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig"></a><span data-ttu-id="2c8d3-163">soubor Web.config</span><span class="sxs-lookup"><span data-stu-id="2c8d3-163">web.config</span></span>

<span data-ttu-id="2c8d3-164">*Web.config* souboru primárním účelem je konfigurace [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-164">The *web.config* file's primary purpose is to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="2c8d3-165">Volitelně ji mohou zadat další nastavení konfigurace služby IIS.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-165">It may optionally provide additional IIS configuration settings.</span></span> <span data-ttu-id="2c8d3-166">Vytváření, transformace a publikování *web.config* zpracováván pomocí .NET SDK webového jádra (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-166">Creating, transforming, and publishing *web.config* is handled by the .NET Core Web SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="2c8d3-167">Sada SDK je nastavena v horní části souboru projektu `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-167">The SDK is set  at the top of the project file, `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span></span> <span data-ttu-id="2c8d3-168">Zabránit transformace sady SDK *web.config* soubor, přidejte  **\<IsTransformWebConfigDisabled >** vlastnost k souboru projektu s nastavením `true`:</span><span class="sxs-lookup"><span data-stu-id="2c8d3-168">To prevent the SDK from transforming the *web.config* file, add the **\<IsTransformWebConfigDisabled>** property to the project file with a setting of `true`:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="2c8d3-169">Pokud *web.config* soubor je v projektu, převede se správnou *processPath* a *argumenty* nakonfigurovat [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) a přesunut do [publikovaná výstup](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-169">If a *web.config* file is in the project, it's transformed with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span> <span data-ttu-id="2c8d3-170">Transformace není změnit nastavení konfigurace služby IIS v souboru.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-170">The transformation doesn't modify IIS configuration settings in the file.</span></span>

### <a name="webconfig-location"></a><span data-ttu-id="2c8d3-171">umístění souboru Web.config</span><span class="sxs-lookup"><span data-stu-id="2c8d3-171">web.config location</span></span>

<span data-ttu-id="2c8d3-172">Aplikace .NET core hostují přes reverzní proxy server mezi službou IIS a Kestrel server.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-172">.NET Core apps are hosted via a reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="2c8d3-173">Chcete-li vytvořit reverzní proxy server, *web.config* souboru musí být přítomné v obsahu kořenovou cestu (obvykle aplikace základní cesta) nasazené aplikace, které je fyzická cesta web zadat do služby IIS.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-173">In order to create the reverse proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app, which is the website physical path provided to IIS.</span></span> <span data-ttu-id="2c8d3-174">*Web.config* soubor je vyžadován v kořenovém adresáři aplikace, aby se daly publikovat víc aplikací pomocí nástroje nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-174">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="2c8d3-175">Citlivé soubory existují na fyzickou cestu aplikace, včetně podsložek, jako například  *\<assembly_name >. runtimeconfig.json*,  *\<assembly_name > .xml* (XML Dokumentační komentáře), a  *\<assembly_name >. deps.json*.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-175">Sensitive files exist on the app's physical path, including subfolders, such as *\<assembly_name>.runtimeconfig.json*, *\<assembly_name>.xml* (XML Documentation comments), and *\<assembly_name>.deps.json*.</span></span> <span data-ttu-id="2c8d3-176">Když *web.config* souboru je k dispozici a konfiguruje lokalitu, služba IIS zabraňuje zpracování těchto citlivé soubory.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-176">When the *web.config* file is present and configures the site, IIS prevents these sensitive files from being served.</span></span> <span data-ttu-id="2c8d3-177">**Proto je důležité, který *web.config* soubor není ať už náhodně, přejmenovat nebo odebrat z nasazení.**</span><span class="sxs-lookup"><span data-stu-id="2c8d3-177">**Therefore, it's important that the *web.config* file isn't accidently renamed or removed from the deployment.**</span></span>

## <a name="create-the-iis-website"></a><span data-ttu-id="2c8d3-178">Vytvoření webu služby IIS</span><span class="sxs-lookup"><span data-stu-id="2c8d3-178">Create the IIS Website</span></span>

1. <span data-ttu-id="2c8d3-179">V cílovém systému služby IIS, vytvořte složku tak, aby obsahovala aplikace publikované složek a souborů, které jsou popsány v [adresářovou strukturu](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-179">On the target IIS system, create a folder to contain the app's published folders and files, which are described in [Directory Structure](xref:host-and-deploy/directory-structure).</span></span>

2. <span data-ttu-id="2c8d3-180">Ve složce, vytvoření *protokoly* složku pro uložení protokoly stdout, pokud je povoleno protokolování stdout.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-180">Within the folder, create a *logs* folder to hold stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="2c8d3-181">Pokud je aplikace nasazena pomocí *protokoly* složku v datové části, tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-181">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="2c8d3-182">Pokyny, jak provádět MSBuild vytvořit *protokoly* složky, najdete v článku [adresářovou strukturu](xref:host-and-deploy/directory-structure) tématu.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-182">For instructions on how to make MSBuild create the *logs* folder, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

3. <span data-ttu-id="2c8d3-183">V **Správce služby IIS**, vytvoření nového webu.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-183">In **IIS Manager**, create a new website.</span></span> <span data-ttu-id="2c8d3-184">Zadejte **název lokality** a nastavte **fyzická cesta** do složky pro nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-184">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="2c8d3-185">Zadejte **vazby** konfigurace a vytvořit web.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-185">Provide the **Binding** configuration and create the website.</span></span>

4. <span data-ttu-id="2c8d3-186">Nastavte fond aplikací **bez spravovaného kódu**.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-186">Set the application pool to **No Managed Code**.</span></span> <span data-ttu-id="2c8d3-187">ASP.NET Core spouští v samostatném procesu a spravuje modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-187">ASP.NET Core runs in a separate process and manages the runtime.</span></span>

5. <span data-ttu-id="2c8d3-188">Otevřete **přidat web** okno.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-188">Open the **Add Website** window.</span></span>

   !["Přidat web, vyberte z kontextové nabídky weby.](index/_static/add-website-context-menu-ws2016.png)

6. <span data-ttu-id="2c8d3-190">Konfigurace webu.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-190">Configure the website.</span></span>

   ![Zadejte název lokality, fyzickou cestu a název hostitele v kroku přidat web.](index/_static/add-website-ws2016.png)

7. <span data-ttu-id="2c8d3-192">V **fondy aplikací** panelech, otevřete **upravit fond aplikací** okno kliknutím pravým tlačítkem myši na fond aplikací webu a výběrem **základní nastavení...**  z místní nabídky.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-192">In the **Application Pools** panel, open the **Edit Application Pool** window by right-clicking on the website's app pool and selecting **Basic Settings...** from the popup menu.</span></span>

   ![Vyberte základní nastavení v kontextové nabídce fondu aplikací.](index/_static/apppools-basic-settings-ws2016.png)

8. <span data-ttu-id="2c8d3-194">Nastavte **verze modulu .NET CLR** k **bez spravovaného kódu**.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-194">Set the **.NET CLR version** to **No Managed Code**.</span></span>

   ![Nastavit bez spravovaného kódu pro verze .NET CLR.](index/_static/edit-apppool-ws2016.png)
     
    <span data-ttu-id="2c8d3-196">Poznámka: Nastavení **verze modulu .NET CLR** k **bez spravovaného kódu** je volitelný.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-196">Note: Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span> <span data-ttu-id="2c8d3-197">ASP.NET Core není spoléhají na načítání plochy CLR.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-197">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span>

9. <span data-ttu-id="2c8d3-198">Potvrďte, že identita procesu modelu má příslušná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-198">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="2c8d3-199">Pokud výchozí identitu fondu aplikací (**Model procesu** > **Identity**) se změní z **ApplicationPoolIdentity** na jinou identitu, ověřte, zda novou identitu má požadovaná oprávnění k přístupu aplikace složka, databáze a další požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-199">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span>
   
## <a name="deploy-the-app"></a><span data-ttu-id="2c8d3-200">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="2c8d3-200">Deploy the app</span></span>

<span data-ttu-id="2c8d3-201">Nasazení aplikace na složku vytvořit v cílovém systému služby IIS.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-201">Deploy the app to the folder created on the target IIS system.</span></span> <span data-ttu-id="2c8d3-202">[Nasazení webové](/iis/publish/using-web-deploy/introduction-to-web-deploy) je doporučené mechanismus pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-202">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

<span data-ttu-id="2c8d3-203">Potvrďte, že není spuštěn publikované aplikace pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-203">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="2c8d3-204">Soubory *publikování* složky jsou zamčené, když aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-204">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="2c8d3-205">Uzamčené soubory nelze přepsat.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-205">Locked files can't be overwritten.</span></span> <span data-ttu-id="2c8d3-206">Chcete-li uvolnit uzamčené soubory v nasazení, zastavte fondu aplikací:</span><span class="sxs-lookup"><span data-stu-id="2c8d3-206">To release locked files in a deployment, stop the app pool:</span></span>

* <span data-ttu-id="2c8d3-207">Ručně v Správce služby IIS na serveru.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-207">Manually in the IIS Manager on the server.</span></span>
* <span data-ttu-id="2c8d3-208">Pomocí nástroje nasazení webu a odkazování na `Microsoft.NET.Sdk.Web` v souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-208">Using Web Deploy and referencing `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="2c8d3-209">*App_offline.htm* soubor je umístěn v kořenovém adresáři webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-209">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="2c8d3-210">Když se soubor nachází, modul ASP.NET Core řádně ukončí aplikaci a slouží *app_offline.htm* souboru během nasazení.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-210">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="2c8d3-211">Další informace najdete v tématu [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-211">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>
* <span data-ttu-id="2c8d3-212">Pomocí prostředí PowerShell zastavte a restartujte fond aplikací (vyžaduje rozhraní PowerShell 5 nebo novější):</span><span class="sxs-lookup"><span data-stu-id="2c8d3-212">Use PowerShell to stop and restart the app pool (requires PowerShell 5 or later):</span></span>
  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="2c8d3-213">Nasazení webu pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c8d3-213">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="2c8d3-214">Najdete v článku [profilů publikování sady Visual Studio pro nasazování aplikací ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) tématu se dozvíte, jak vytvořit profil publikování pro použití pomocí nástroje nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-214">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="2c8d3-215">Pokud poskytovatel hostingu poskytuje profilu publikování nebo podpora pro vytváření jeden, stáhněte si svůj profil a importujte jej pomocí sady Visual Studio **publikovat** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-215">If the hosting provider supplies a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Dialogové okno stránky publikování](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="2c8d3-217">Nasazení webu mimo Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c8d3-217">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="2c8d3-218">[Nasazení webové](/iis/publish/using-web-deploy/introduction-to-web-deploy) lze také použít mimo Visual Studio z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-218">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="2c8d3-219">Další informace najdete v tématu [nástroj pro nasazení webu](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-219">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="2c8d3-220">Alternativy k webového nasazení</span><span class="sxs-lookup"><span data-stu-id="2c8d3-220">Alternatives to Web Deploy</span></span>

<span data-ttu-id="2c8d3-221">Přesunout aplikaci k hostování systému, třeba Xcopy, Robocopy nebo prostředí PowerShell můžete použijte některou z několika metod.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-221">Use any of several methods to move the app to the hosting system, such as Xcopy, Robocopy, or PowerShell.</span></span> <span data-ttu-id="2c8d3-222">Visual Studio uživatelé mohou využít [publikování ukázky](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-222">Visual Studio users may use the [Publish Samples](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="2c8d3-223">Přejděte na web</span><span class="sxs-lookup"><span data-stu-id="2c8d3-223">Browse the website</span></span>

![V prohlížeči Microsoft Edge načetl úvodní stránce služby IIS.](index/_static/browsewebsite.png)

## <a name="data-protection"></a><span data-ttu-id="2c8d3-225">Ochrana dat</span><span class="sxs-lookup"><span data-stu-id="2c8d3-225">Data protection</span></span>

<span data-ttu-id="2c8d3-226">Ochrana dat je využíván několika middlewares ASP.NET, včetně těch, které používané v ověřování.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-226">Data Protection is used by several ASP.NET middlewares, including those used in authentication.</span></span> <span data-ttu-id="2c8d3-227">I v případě, že rozhraní Data Protection API nejsou volat z kódu uživatele, musí se nakonfigurovat ochranu dat pomocí skriptu pro nasazení nebo v uživatelském kódu pro vytvoření trvalého úložiště klíčů.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-227">Even if Data Protection APIs aren't called from user's code, Data Protection should be configured with a deployment script or in user code to create a persistent key store.</span></span> <span data-ttu-id="2c8d3-228">Pokud není nakonfigurovaná ochrana dat, jsou klíče uložené v paměti a zahozené během restartování aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-228">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="2c8d3-229">Pokud se Správce klíčů je uložené v paměti po restartování aplikace:</span><span class="sxs-lookup"><span data-stu-id="2c8d3-229">If the keyring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="2c8d3-230">Všechny forms ověřovací tokeny jsou zneplatněny.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-230">All forms authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="2c8d3-231">Uživatelé musí znovu přihlásit na jejich další požadavek.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-231">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="2c8d3-232">Všechna data chráněné pomocí Správce klíčů lze už dešifrovat.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-232">Any data protected with the keyring can no longer be decrypted.</span></span>

<span data-ttu-id="2c8d3-233">Ke konfiguraci ochrany dat v rámci služby IIS, použijte **jeden** z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="2c8d3-233">To configure Data Protection under IIS, use **one** of the following approaches:</span></span>

### <a name="create-a-data-protection-registry-hive"></a><span data-ttu-id="2c8d3-234">Vytvoření podregistr registru ochrany dat</span><span class="sxs-lookup"><span data-stu-id="2c8d3-234">Create a Data Protection Registry Hive</span></span>

<span data-ttu-id="2c8d3-235">Klíče ochrany dat používá aplikace ASP.NET jsou uloženy v registru podregistrů externí aplikací.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-235">Data Protection keys used by ASP.NET apps are stored in registry hives external to the apps.</span></span> <span data-ttu-id="2c8d3-236">Chcete-li zachovat klíče pro danou aplikaci, vytvořte podregistru pro fond aplikací.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-236">To persist the keys for a given app, create a registry hive for the app pool.</span></span>

<span data-ttu-id="2c8d3-237">Pro samostatné instalace služby IIS – webová farma [skript prostředí PowerShell AutoGenKeys.ps1 zřízení ochrany dat](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) lze použít pro každý fond aplikací používat s aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-237">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="2c8d3-238">Tento skript vytvoří speciální klíč v registru HKLM, který je ACLed pouze pro účet pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-238">This script creates a special registry key in the HKLM registry that is ACLed only to the worker process account.</span></span> <span data-ttu-id="2c8d3-239">Klíče jsou zašifrovaná přinejmenším pomocí rozhraní DPAPI klíčem celého systému.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-239">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

<span data-ttu-id="2c8d3-240">Ve scénářích webové farmy může být aplikace nakonfigurován pro použijte cestu UNC k ukládání klíčů ochrany jeho data.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-240">In web farm scenarios, an app can be configured to use a UNC path to store its data protection keyring.</span></span> <span data-ttu-id="2c8d3-241">Ve výchozím nastavení nejsou klíče ochrany dat šifrována.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-241">By default, the data protection keys are not encrypted.</span></span> <span data-ttu-id="2c8d3-242">Zkontrolujte, zda jsou omezená na účet systému Windows, které aplikace běží jako soubor oprávnění pro tyto sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-242">Ensure that the file permissions for such a share are limited to the Windows account the app runs as.</span></span> <span data-ttu-id="2c8d3-243">Kromě toho, X509 certifikát můžete použít k ochraně klíče v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-243">In addition, an X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="2c8d3-244">Vezměte v úvahu mechanismus, aby uživatelé mohli nahrát certifikátů: místní certifikáty do důvěryhodného certifikátu uživatele, ukládání a ujistěte se, jsou k dispozici na všech počítačích, kde je spuštěna aplikace uživatele.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-244">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="2c8d3-245">V tématu [konfiguraci ochrany dat](xref:security/data-protection/configuration/overview) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-245">See [Configuring Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a><span data-ttu-id="2c8d3-246">Nakonfigurujte fond aplikací služby IIS načíst profil uživatele</span><span class="sxs-lookup"><span data-stu-id="2c8d3-246">Configure the IIS Application Pool to load the user profile</span></span>

<span data-ttu-id="2c8d3-247">Toto nastavení je ve **Model procesu** oddílu pod **Upřesnit nastavení** pro fond aplikací.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-247">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="2c8d3-248">Nastavit načíst profil uživatele na `True`.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-248">Set Load User Profile to `True`.</span></span> <span data-ttu-id="2c8d3-249">To ukládá klíče v adresáři profilu uživatele a chrání je pomocí rozhraní DPAPI klíčem specifické pro uživatelský účet použitý pro fond aplikací.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-249">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used for the app pool.</span></span>

### <a name="use-the-file-system-as-a-key-ring-store"></a><span data-ttu-id="2c8d3-250">Pomocí systému souborů jako úložiště prstenec klíč</span><span class="sxs-lookup"><span data-stu-id="2c8d3-250">Use the file system as a key ring store</span></span>

<span data-ttu-id="2c8d3-251">Upravit kód aplikace, který [pomocí systému souborů jako úložiště prstenec klíč](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-251">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="2c8d3-252">Použití na X509 certifikát k ochraně Správce klíčů a ujistěte se, certifikát je důvěryhodný certifikát.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-252">Use an X509 certificate to protect the keyring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="2c8d3-253">Pokud je certifikát podepsaný sám sebou, umístěte certifikát v úložišti Důvěryhodné kořenové.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-253">If it's a self signed certificate, place the certificate in the Trusted Root store.</span></span>

<span data-ttu-id="2c8d3-254">Při použití IIS ve webové farmě:</span><span class="sxs-lookup"><span data-stu-id="2c8d3-254">When using IIS in a web farm:</span></span>

* <span data-ttu-id="2c8d3-255">Použijte sdílené složky, ke kterému mají přístup všechny počítače.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-255">Use a file share that all machines can access.</span></span>
* <span data-ttu-id="2c8d3-256">Nasazení X509 certifikát pro každý počítač.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-256">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="2c8d3-257">Konfigurace [ochrany dat v kódu](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-257">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

### <a name="set-a-machine-wide-policy-for-data-protection"></a><span data-ttu-id="2c8d3-258">Nastavit zásady celého systému ochrany dat</span><span class="sxs-lookup"><span data-stu-id="2c8d3-258">Set a machine-wide policy for data protection</span></span>

<span data-ttu-id="2c8d3-259">Systém ochrany dat má omezenou podporu pro nastavení výchozí [celého zásad](xref:security/data-protection/configuration/machine-wide-policy) pro všechny aplikace, které využívají rozhraní Data Protection API.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-259">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="2c8d3-260">Najdete v článku [ochrany dat](xref:security/data-protection/index) dokumentace pro další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-260">See the [data protection](xref:security/data-protection/index) documentation for more details.</span></span>

## <a name="configuration-of-sub-applications"></a><span data-ttu-id="2c8d3-261">Konfiguraci dílčí aplikací</span><span class="sxs-lookup"><span data-stu-id="2c8d3-261">Configuration of sub-applications</span></span>

<span data-ttu-id="2c8d3-262">Dílčí aplikace přidají pod kořenové aplikace by neměla zahrnovat modul ASP.NET Core jako obslužná rutina.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-262">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="2c8d3-263">Pokud je modul přidán jako obslužná rutina v dílčí aplikaci *web.config* souboru, 500.19 (vnitřní chyba serveru) odkazující na vadný konfigurační soubor je oznámených při pokusu o vyhledání aplikace sub.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-263">If the module is added as a handler in a sub-app's *web.config* file, a 500.19 (Internal Server Error) referencing the faulty config file is received when attempting to browse the sub-app.</span></span> <span data-ttu-id="2c8d3-264">Následující příklad ukazuje obsah publikovaný *web.config* soubor pro dílčí ASP.NET Core-aplikace:</span><span class="sxs-lookup"><span data-stu-id="2c8d3-264">The following example shows the contents of a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="2c8d3-265">Při hostování ASP.NET Core dílčí aplikaci pod aplikace ASP.NET Core, v aplikaci dílčí explicitně odebrat zděděné obslužná rutina *web.config* souboru:</span><span class="sxs-lookup"><span data-stu-id="2c8d3-265">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="2c8d3-266">Další informace o konfiguraci technologie ASP.NET základní modul, najdete v článku [Úvod k modulu jádra ASP.NET](xref:fundamentals/servers/aspnet-core-module) tématu a [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-266">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="2c8d3-267">Konfigurace služby IIS pomocí souboru web.config</span><span class="sxs-lookup"><span data-stu-id="2c8d3-267">Configuration of IIS with web.config</span></span>

<span data-ttu-id="2c8d3-268">Konfigurace služby IIS je ovlivněno  **\<system.webServer >** části *web.config* pro tyto funkce služby IIS, která se týkají konfigurace reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-268">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="2c8d3-269">Pokud je služba IIS konfigurována na úrovni systému použití dynamické komprese, toto nastavení je možné zakázat aplikace pomocí  **\<urlCompression >** element v dané aplikaci *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-269">If IIS is configured at the system level to use dynamic compression, that setting can be disabled for an app with the **\<urlCompression>** element in the app's *web.config* file.</span></span> <span data-ttu-id="2c8d3-270">Další informace najdete v tématu [odkaz Konfigurace pro \<system.webServer >](https://docs.microsoft.com/iis/configuration/system.webServer/), [odkazu na modul Konfigurace ASP.NET Core](xref:host-and-deploy/aspnet-core-module) a [pomocí moduly služby IIS s prostředím ASP. Základní NET](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-270">For more information, see the [configuration reference for \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module) and [Using IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="2c8d3-271">Pokud je potřeba nastavit proměnné prostředí pro jednotlivé aplikace spuštěných ve fondech izolované aplikace (podporuje se ve službě IIS 10.0 +), najdete *příkazu AppCmd.exe* části [proměnné prostředí \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) téma v IIS referenční dokumentace.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-271">If there's a need to set environment variables for individual apps running in isolated app pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="2c8d3-272">Konfigurační oddíly souboru Web.config</span><span class="sxs-lookup"><span data-stu-id="2c8d3-272">Configuration sections of web.config</span></span>

<span data-ttu-id="2c8d3-273">Části Configruation aplikace rozhraní ASP.NET v *web.config* nejsou používány nástrojem aplikace ASP.NET Core pro konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="2c8d3-273">Configruation sections of ASP.NET Framework apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="2c8d3-274">**\<System.Web >**</span><span class="sxs-lookup"><span data-stu-id="2c8d3-274">**\<system.web>**</span></span>
* <span data-ttu-id="2c8d3-275">**\<appSettings >**</span><span class="sxs-lookup"><span data-stu-id="2c8d3-275">**\<appSettings>**</span></span>
* <span data-ttu-id="2c8d3-276">**\<connectionStrings >**</span><span class="sxs-lookup"><span data-stu-id="2c8d3-276">**\<connectionStrings>**</span></span>
* <span data-ttu-id="2c8d3-277">**\<umístění >**</span><span class="sxs-lookup"><span data-stu-id="2c8d3-277">**\<location>**</span></span>

<span data-ttu-id="2c8d3-278">Aplikace ASP.NET Core jsou konfigurováni pomocí jiných poskytovatelů konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-278">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="2c8d3-279">Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="2c8d3-279">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="2c8d3-280">Fondy aplikací</span><span class="sxs-lookup"><span data-stu-id="2c8d3-280">Application Pools</span></span>

<span data-ttu-id="2c8d3-281">Při hostování více webů na jednom systému, izolace aplikace od sebe navzájem spuštěním každou aplikaci ve vlastním fondu aplikací.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-281">When hosting multiple websites on a single system, isolate the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="2c8d3-282">Služby IIS **přidat web** dialogovém okně výchozí hodnoty pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-282">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="2c8d3-283">Když **název lokality** je zadaný text se automaticky přenese na **fond aplikací** textové pole.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-283">When **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="2c8d3-284">Nový fond aplikací se vytvoří při přidání webu pomocí názvu serveru.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-284">A new app pool is created using the site name when the website is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="2c8d3-285">Identita fondu aplikací</span><span class="sxs-lookup"><span data-stu-id="2c8d3-285">Application Pool Identity</span></span>

<span data-ttu-id="2c8d3-286">Účet identity fondu aplikací umožňuje aplikaci, běh jedinečný účet, aniž by museli vytvářet a spravovat domény nebo místní účty.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-286">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="2c8d3-287">Ve službě IIS 8.0 + procesů Worker Správce služby IIS (WAS) vytvoří virtuální účet s názvem nového fondu aplikací a aplikace spouští pracovní procesy fondu pod tímto účtem ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-287">On IIS 8.0+, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="2c8d3-288">V konzole pro správu služby IIS v části **Upřesnit nastavení** pro fond aplikací, zkontrolujte, zda **Identity** je nastavený na použití **ApplicationPoolIdentity**:</span><span class="sxs-lookup"><span data-stu-id="2c8d3-288">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Dialogové okno Upřesnit nastavení fondu aplikací](index/_static/apppool-identity.png)

<span data-ttu-id="2c8d3-290">Proces správy služby IIS vytvoří zabezpečeného identifikátoru se název fondu aplikací v zabezpečení systému Windows.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-290">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="2c8d3-291">Prostředky lze zabezpečit pomocí tuto identitu; Tato identita však není skutečné uživatelský účet a nezobrazí v konzole pro správu uživatelů systému Windows.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-291">Resources can be secured by using this identity; however, this identity isn't a real user account and won't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="2c8d3-292">Pokud pracovní proces služby IIS vyžaduje přístup k aplikaci se zvýšeným oprávněním, upravte seznam řízení přístupu (ACL) pro adresář obsahující aplikaci:</span><span class="sxs-lookup"><span data-stu-id="2c8d3-292">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="2c8d3-293">Otevřete Průzkumníka Windows a přejděte do adresáře.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-293">Open Windows Explorer and navigate to the directory.</span></span>

2. <span data-ttu-id="2c8d3-294">Klikněte pravým tlačítkem na adresář a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-294">Right-click on the directory and select **Properties**.</span></span>

3. <span data-ttu-id="2c8d3-295">V části **zabezpečení** vyberte **upravit** tlačítko a potom **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-295">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

4. <span data-ttu-id="2c8d3-296">Vyberte **umístění** tlačítko a musí být vybrána systému.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-296">Select the **Locations** button and make sure the system is selected.</span></span>

5. <span data-ttu-id="2c8d3-297">Zadejte **IIS AppPool\DefaultAppPool** v **zadejte názvy objektů k výběru** textové pole.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-297">Enter **IIS AppPool\DefaultAppPool** in **Enter the object names to select** textbox.</span></span>

   ![Vyberte uživatele nebo skupiny dialogové okno pro složce aplikace](index/_static/select-users-or-groups-1.png)

6. <span data-ttu-id="2c8d3-299">Vyberte **Kontrola názvů** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-299">Select the **Check Names** button.</span></span> <span data-ttu-id="2c8d3-300">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="2c8d3-300">Select **OK**.</span></span>

   ![Vyberte uživatele nebo skupiny dialogové okno pro složce aplikace](index/_static/select-users-or-groups-2.png)

<span data-ttu-id="2c8d3-302">To můžete provést také pomocí příkazového řádku pomocí **ICACLS** nástroje:</span><span class="sxs-lookup"><span data-stu-id="2c8d3-302">This can also be accomplished via a command prompt using the **ICACLS** tool:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a><span data-ttu-id="2c8d3-303">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2c8d3-303">Additional resources</span></span>

* [<span data-ttu-id="2c8d3-304">Řešení potíží s ASP.NET Core ve službě IIS</span><span class="sxs-lookup"><span data-stu-id="2c8d3-304">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="2c8d3-305">Běžné chyby referenční dokumentace pro Azure App Service a IIS s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2c8d3-305">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="2c8d3-306">Úvod do modulu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2c8d3-306">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="2c8d3-307">Odkaz na konfiguraci základní modul ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2c8d3-307">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="2c8d3-308">Moduly služby IIS pomocí ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2c8d3-308">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="2c8d3-309">Úvod do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2c8d3-309">Introduction to ASP.NET Core</span></span>](../index.md)
* [<span data-ttu-id="2c8d3-310">Lokality oficiální Microsoft IIS</span><span class="sxs-lookup"><span data-stu-id="2c8d3-310">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="2c8d3-311">Microsoft TechNet Library: Windows Server</span><span class="sxs-lookup"><span data-stu-id="2c8d3-311">Microsoft TechNet Library: Windows Server</span></span>](https://docs.microsoft.com/windows-server/windows-server-versions)
