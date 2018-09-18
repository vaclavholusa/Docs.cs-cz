---
title: Implementací webového serveru v ASP.NET Core
author: rick-anderson
description: Zjišťování webové servery přes Kestrel a HTTP.sys pro ASP.NET Core. Zjistěte, jak vybrat server a kdy použít reverzní proxy server.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/13/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 0f1460af5bc1cd879ff11e43775ac16ca36b150e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011752"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="10e38-104">Implementací webového serveru v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="10e38-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="10e38-105">Podle [Petr Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), a [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="10e38-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="10e38-106">Aplikace ASP.NET Core spouští implementaci serveru HTTP v procesu.</span><span class="sxs-lookup"><span data-stu-id="10e38-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="10e38-107">Implementace server přijímá požadavky protokolu HTTP a poskytuje je na aplikaci jako sady [funkce požadavků](xref:fundamentals/request-features) složený do [HttpContext](/dotnet/api/system.web.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="10e38-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an [HttpContext](/dotnet/api/system.web.httpcontext).</span></span>

<span data-ttu-id="10e38-108">ASP.NET Core se celá dodává dvě implementace serveru:</span><span class="sxs-lookup"><span data-stu-id="10e38-108">ASP.NET Core ships two server implementations:</span></span>

* <span data-ttu-id="10e38-109">[Kestrel](xref:fundamentals/servers/kestrel) je výchozí, server HTTP pro různé platformy pro ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10e38-109">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="10e38-110">[Ovladač HTTP.sys](xref:fundamentals/servers/httpsys) je na základě protokolu HTTP jenom pro Windows server [ovladač HTTP.sys jádra a rozhraní API serveru HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="10e38-110">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="10e38-111">(Ovladač HTTP.sys se nazývá [WebListener](xref:fundamentals/servers/weblistener) v ASP.NET Core 1.x.)</span><span class="sxs-lookup"><span data-stu-id="10e38-111">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

## <a name="kestrel"></a><span data-ttu-id="10e38-112">Kestrel</span><span class="sxs-lookup"><span data-stu-id="10e38-112">Kestrel</span></span>

<span data-ttu-id="10e38-113">Kestrel je výchozí webový server, která je součástí šablony projektů ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10e38-113">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="10e38-114">Kestrel lze použít samostatně nebo se *reverzní proxy server*, jako je například Apache, IIS nebo Nginx.</span><span class="sxs-lookup"><span data-stu-id="10e38-114">Kestrel can be used by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="10e38-115">Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je na Kestrel po některé předběžného zpracování.</span><span class="sxs-lookup"><span data-stu-id="10e38-115">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel komunikuje přímo s Internetu bez reverzní proxy server](kestrel/_static/kestrel-to-internet2.png)

![Kestrel nepřímo komunikuje přes Internet prostřednictvím reverzního proxy serveru, jako je například Apache, IIS nebo Nginx](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="10e38-118">Buď konfiguraci&mdash;s nebo bez něj reverzní proxy server&mdash;je platný a podporované konfigurace pro hostování pro ASP.NET Core 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="10e38-118">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="10e38-119">Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="10e38-119">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="10e38-120">Pokud aplikace přijímá jenom požadavky z interní sítě, je možné Kestrel samostatně.</span><span class="sxs-lookup"><span data-stu-id="10e38-120">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel komunikuje přímo s interní sítě](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="10e38-122">Pokud aplikace je přístupný z Internetu, Kestrel musíte použít službu IIS, serveru Nginx nebo Apache jako *reverzní proxy server*.</span><span class="sxs-lookup"><span data-stu-id="10e38-122">If the app is exposed to the Internet, Kestrel must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="10e38-123">Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je na Kestrel po některé předběžné zpracování, jak je znázorněno v následujícím diagramu:</span><span class="sxs-lookup"><span data-stu-id="10e38-123">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram:</span></span>

![Kestrel nepřímo komunikuje přes Internet prostřednictvím reverzního proxy serveru, jako je například Apache, IIS nebo Nginx](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="10e38-125">Nejdůležitější důvod pomocí reverzního proxy serveru pro nasazení hraniční (vystavené pro provoz z Internetu) je zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="10e38-125">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="10e38-126">Verze 1.x Kestrel nemají funkcí důležité zabezpečení, ochranu před útoky z Internetu.</span><span class="sxs-lookup"><span data-stu-id="10e38-126">The 1.x versions of Kestrel don't have important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="10e38-127">To zahrnuje, ale není omezený na, odpovídající časové limity, žádost o velikosti omezení a omezení počtu souběžných připojení.</span><span class="sxs-lookup"><span data-stu-id="10e38-127">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="10e38-128">Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="10e38-128">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

<span data-ttu-id="10e38-129">Apache, IIS a serveru Nginx nelze použít bez Kestrel nebo [implementace vlastního serveru](#custom-servers).</span><span class="sxs-lookup"><span data-stu-id="10e38-129">IIS, Nginx, and Apache can't be used without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="10e38-130">ASP.NET Core je navržená ke spuštění ve svém vlastním procesu tak, aby se může chovat konzistentně napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="10e38-130">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="10e38-131">Apache, IIS a serveru Nginx určovat vlastní spouštěcí procedura a prostředí.</span><span class="sxs-lookup"><span data-stu-id="10e38-131">IIS, Nginx, and Apache dictate their own startup procedure and environment.</span></span> <span data-ttu-id="10e38-132">K použití těchto technologií server přímo, ASP.NET Core potřebovat umožní reagovat na požadavky na každém serveru.</span><span class="sxs-lookup"><span data-stu-id="10e38-132">To use these server technologies directly, ASP.NET Core would need to adapt to the requirements of each server.</span></span> <span data-ttu-id="10e38-133">Použití implementace webového serveru, jako je například Kestrel, ASP.NET Core má kontrolu nad procesu spouštění a prostředí, když jsou hostované na jiný server technologie.</span><span class="sxs-lookup"><span data-stu-id="10e38-133">Using a web server implementation, such as Kestrel, ASP.NET Core has control over the startup process and environment when hosted on different server technologies.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="10e38-134">IIS s Kestrel</span><span class="sxs-lookup"><span data-stu-id="10e38-134">IIS with Kestrel</span></span>

<span data-ttu-id="10e38-135">Při použití [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) nebo [služby IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) jako reverzní proxy server pro ASP.NET Core, ASP.NET Core aplikace běží v procesu nezávisle na pracovní proces služby IIS.</span><span class="sxs-lookup"><span data-stu-id="10e38-135">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="10e38-136">V procesu služby IIS [modul ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) koordinuje vztah reverzního proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="10e38-136">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="10e38-137">Primární funkce modul ASP.NET Core jsou ke spuštění aplikace ASP.NET Core, restartujte aplikaci, pokud ho dojde k chybě a přesměrování provozu HTTP na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="10e38-137">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="10e38-138">Další informace najdete v tématu [modul ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="10e38-138">For more information, see [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> 

### <a name="nginx-with-kestrel"></a><span data-ttu-id="10e38-139">Server Nginx s Kestrel</span><span class="sxs-lookup"><span data-stu-id="10e38-139">Nginx with Kestrel</span></span>

<span data-ttu-id="10e38-140">Informace o tom, jak použít Nginx jako reverzní proxy server pro Kestrel v Linuxu najdete v tématu [hostování v Linuxu se serverem Nginx na](xref:host-and-deploy/linux-nginx).</span><span class="sxs-lookup"><span data-stu-id="10e38-140">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="10e38-141">Apache s Kestrel</span><span class="sxs-lookup"><span data-stu-id="10e38-141">Apache with Kestrel</span></span>

<span data-ttu-id="10e38-142">Informace o tom, jak používat Apache na platformě Linux jako reverzní proxy server pro Kestrel najdete v tématu [hostitele v Linuxu pomocí Apache](xref:host-and-deploy/linux-apache).</span><span class="sxs-lookup"><span data-stu-id="10e38-142">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="10e38-143">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="10e38-143">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="10e38-144">Pokud aplikace ASP.NET Core běží na Windows, ovladač HTTP.sys se o alternativu k Kestrel.</span><span class="sxs-lookup"><span data-stu-id="10e38-144">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="10e38-145">Kestrel obecně se doporučuje pro zajištění nejlepšího výkonu.</span><span class="sxs-lookup"><span data-stu-id="10e38-145">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="10e38-146">Ovladač HTTP.sys lze použít v situacích, kdy aplikace je přístupný z Internetu a požadované funkce jsou podporovány, ale ne Kestrel HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="10e38-146">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="10e38-147">Informace o souboru HTTP.sys, naleznete v tématu [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="10e38-147">For information on HTTP.sys, see [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

![Ovladač HTTP.sys komunikuje přímo s Internetem](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="10e38-149">Ovladač HTTP.sys lze použít také pro aplikace, které jsou přístupné pouze k interní síti.</span><span class="sxs-lookup"><span data-stu-id="10e38-149">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![Ovladač HTTP.sys komunikuje přímo s interní sítě](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="10e38-151">Název souboru HTTP.sys [WebListener](xref:fundamentals/servers/weblistener) v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="10e38-151">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="10e38-152">Pokud aplikace ASP.NET Core běží na Windows, WebListener se o alternativu pro scénářů, kdy není k dispozici pro hostování aplikací služby IIS.</span><span class="sxs-lookup"><span data-stu-id="10e38-152">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![Weblistener komunikuje přímo s Internetem](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="10e38-154">WebListener lze také místo Kestrel pro aplikace, které jsou přístupné pouze k interní síti, pokud je to nutné, že funkce jsou podporovány, ale ne Kestrel WebListener.</span><span class="sxs-lookup"><span data-stu-id="10e38-154">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="10e38-155">Informace o WebListener najdete v tématu [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="10e38-155">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![Weblistener komunikuje přímo s interní sítě](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="10e38-157">ASP.NET Core server infrastruktury</span><span class="sxs-lookup"><span data-stu-id="10e38-157">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="10e38-158">[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) k dispozici v `Startup.Configure` zpřístupňuje metodu [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) vlastnost typu [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="10e38-158">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="10e38-159">Přes kestrel a HTTP.sys (WebListener v ASP.NET Core 1.x) vystavit pouze jednu funkci, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ale implementace jiný server může vystavit další funkce.</span><span class="sxs-lookup"><span data-stu-id="10e38-159">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="10e38-160">`IServerAddressesFeature` umožňuje zjistit, port, který obsahuje implementaci serveru vázán v době běhu.</span><span class="sxs-lookup"><span data-stu-id="10e38-160">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="10e38-161">Vlastní servery</span><span class="sxs-lookup"><span data-stu-id="10e38-161">Custom servers</span></span>

<span data-ttu-id="10e38-162">Pokud integrované servery nesplňují požadavky aplikace, implementace vlastního serveru vytvořit.</span><span class="sxs-lookup"><span data-stu-id="10e38-162">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="10e38-163">[Open Web Interface pro .NET (OWIN) průvodce](xref:fundamentals/owin) ukazuje, jak psát [Nowin](https://github.com/Bobris/Nowin)– na základě [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementace.</span><span class="sxs-lookup"><span data-stu-id="10e38-163">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="10e38-164">Pouze funkce rozhraní, které aplikace používá potřeba provádění, když minimálně [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) a [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) musí podporovat.</span><span class="sxs-lookup"><span data-stu-id="10e38-164">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="10e38-165">Při spuštění serveru</span><span class="sxs-lookup"><span data-stu-id="10e38-165">Server startup</span></span>

<span data-ttu-id="10e38-166">Při použití [sady Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), nebo [Visual Studio Code](https://code.visualstudio.com/), server se spustí, když se aplikace spustí na integrované vývojové prostředí ( INTEGROVANÉ VÝVOJOVÉ PROSTŘEDÍ).</span><span class="sxs-lookup"><span data-stu-id="10e38-166">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="10e38-167">V sadě Visual Studio na Windows, můžete profily spouštění použije ke spuštění aplikace a serveru s oběma [služby IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[modul ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) nebo konzoly.</span><span class="sxs-lookup"><span data-stu-id="10e38-167">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="10e38-168">Ve Visual Studio Code, aplikace a serveru jsou spouštěny [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), která aktivuje CoreCLR ladicího programu.</span><span class="sxs-lookup"><span data-stu-id="10e38-168">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="10e38-169">Pomocí sady Visual Studio pro Mac, aplikace a serveru jsou spouštěny [ladicí program Mono konfigurace Soft-režim](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="10e38-169">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="10e38-170">Při spuštění aplikace z příkazového řádku ve složce projektu [dotnet spustit](/dotnet/core/tools/dotnet-run) spustí aplikace a serveru (přes Kestrel a pouze HTTP.sys).</span><span class="sxs-lookup"><span data-stu-id="10e38-170">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="10e38-171">Konfigurace je určena `-c|--configuration` možnost, který je nastaven na hodnotu `Debug` (výchozí) nebo `Release`.</span><span class="sxs-lookup"><span data-stu-id="10e38-171">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="10e38-172">Pokud jsou k dispozici v profily spouštění *launchSettings.json* souboru, použijte `--launch-profile <NAME>` možnost nastavit profil spuštění (například `Development` nebo `Production`).</span><span class="sxs-lookup"><span data-stu-id="10e38-172">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="10e38-173">Další informace najdete v tématu [dotnet spustit](/dotnet/core/tools/dotnet-run) a [vytváření distribučních balíčků .NET Core](/dotnet/core/build/distribution-packaging) témata.</span><span class="sxs-lookup"><span data-stu-id="10e38-173">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="10e38-174">Podpora HTTP/2</span><span class="sxs-lookup"><span data-stu-id="10e38-174">HTTP/2 support</span></span>

<span data-ttu-id="10e38-175">[HTTP/2](https://httpwg.org/specs/rfc7540.html) se podporuje s ASP.NET Core v následujících scénářích nasazení:</span><span class="sxs-lookup"><span data-stu-id="10e38-175">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="10e38-176">Kestrel</span><span class="sxs-lookup"><span data-stu-id="10e38-176">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="10e38-177">Operační systém</span><span class="sxs-lookup"><span data-stu-id="10e38-177">Operating system</span></span>
    * <span data-ttu-id="10e38-178">Windows Server 2012 R2 nebo Windows 8.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="10e38-178">Windows Server 2012 R2/Windows 8.1 or later</span></span>
    * <span data-ttu-id="10e38-179">Linux s OpenSSL 1.0.2 nebo novější (například Ubuntu 16.04 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="10e38-179">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="10e38-180">HTTP/2 budou podporované v systému macOS v budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="10e38-180">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="10e38-181">Cílová architektura: .NET Core 2.2 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="10e38-181">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="10e38-182">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="10e38-182">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="10e38-183">Windows Server 2016 nebo Windows 10 nebo novější</span><span class="sxs-lookup"><span data-stu-id="10e38-183">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="10e38-184">Cílová architektura: není k dispozici pro nasazení souboru HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="10e38-184">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="10e38-185">Služba IIS (v procesu)</span><span class="sxs-lookup"><span data-stu-id="10e38-185">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="10e38-186">Windows Server 2016 nebo Windows 10 nebo novější; IIS 10 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="10e38-186">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="10e38-187">Cílová architektura: .NET Core 2.2 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="10e38-187">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="10e38-188">Služba IIS (out-of-process)</span><span class="sxs-lookup"><span data-stu-id="10e38-188">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="10e38-189">Windows Server 2016 nebo Windows 10 nebo novější; IIS 10 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="10e38-189">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="10e38-190">Edge připojení pomocí protokolu HTTP/2, ale připojení reverzního proxy serveru k Kestrel používá HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="10e38-190">Edge connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="10e38-191">Cílová architektura: není k dispozici pro nasazení na více instancí procesu služby IIS.</span><span class="sxs-lookup"><span data-stu-id="10e38-191">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="10e38-192">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="10e38-192">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="10e38-193">Windows Server 2016 nebo Windows 10 nebo novější</span><span class="sxs-lookup"><span data-stu-id="10e38-193">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="10e38-194">Cílová architektura: není k dispozici pro nasazení souboru HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="10e38-194">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="10e38-195">Služba IIS (out-of-process)</span><span class="sxs-lookup"><span data-stu-id="10e38-195">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="10e38-196">Windows Server 2016 nebo Windows 10 nebo novější; IIS 10 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="10e38-196">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="10e38-197">Edge připojení pomocí protokolu HTTP/2, ale připojení reverzního proxy serveru k Kestrel používá HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="10e38-197">Edge connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="10e38-198">Cílová architektura: není k dispozici pro nasazení na více instancí procesu služby IIS.</span><span class="sxs-lookup"><span data-stu-id="10e38-198">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="10e38-199">Musíte použít připojení HTTP/2 [vyjednávání protokolu v aplikační vrstvě (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) a TLS 1.2 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="10e38-199">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="10e38-200">Další informace najdete v tématech, které se týkají scénářů nasazení serveru.</span><span class="sxs-lookup"><span data-stu-id="10e38-200">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="10e38-201">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="10e38-201">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="10e38-202"><xref:fundamentals/servers/httpsys> (pro technologii ASP.NET Core 1.x, přečtěte si téma <xref:fundamentals/servers/weblistener>)</span><span class="sxs-lookup"><span data-stu-id="10e38-202"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>
