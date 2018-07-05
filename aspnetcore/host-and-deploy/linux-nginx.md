---
title: Hostitele ASP.NET Core v Linuxu se serverem Nginx
author: rick-anderson
description: Další informace o nastavení serveru Nginx jako reverzní proxy server na Ubuntu 16.04 směrovat provoz protokolu HTTP k webové aplikaci ASP.NET Core spuštěnou v prostředí Kestrel.
ms.author: riande
ms.custom: mvc
ms.date: 05/22/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 840a9f98b3409f74b9a41ee24ff7bcb33a875470
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433932"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="75abb-103">Hostitele ASP.NET Core v Linuxu se serverem Nginx</span><span class="sxs-lookup"><span data-stu-id="75abb-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="75abb-104">Podle [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="75abb-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="75abb-105">Tato příručka vysvětluje nastavení prostředí připravené pro produkční prostředí ASP.NET Core na serveru se systémem Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="75abb-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="75abb-106">Tyto pokyny mohou pracovat s novějšími verzemi Ubuntu, ale pokynů nebyly testovány s novější verzí.</span><span class="sxs-lookup"><span data-stu-id="75abb-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

> [!NOTE]
> <span data-ttu-id="75abb-107">Pro Ubuntu 14.04 *supervisord* jako řešení pro monitorování procesu Kestrel se doporučuje.</span><span class="sxs-lookup"><span data-stu-id="75abb-107">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="75abb-108">*systemd* není k dispozici na Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="75abb-108">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="75abb-109">Ubuntu 14.04 pokyny najdete v tématu [předchozí verzi tohoto tématu](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="75abb-109">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="75abb-110">Tento průvodce:</span><span class="sxs-lookup"><span data-stu-id="75abb-110">This guide:</span></span>

* <span data-ttu-id="75abb-111">Umístí stávající aplikace ASP.NET Core za reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="75abb-111">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="75abb-112">Nastaví předávat požadavky na webový server Kestrel reverzního proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="75abb-112">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="75abb-113">Zajišťuje, že webová aplikace spuštěna při spuštění jako démon.</span><span class="sxs-lookup"><span data-stu-id="75abb-113">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="75abb-114">Konfiguruje nástroj pro správu proces usnadňují, restartujte webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="75abb-114">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75abb-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="75abb-115">Prerequisites</span></span>

1. <span data-ttu-id="75abb-116">Přístup k serveru se systémem Ubuntu 16.04 pomocí standardního uživatelského účtu s oprávněními sudo.</span><span class="sxs-lookup"><span data-stu-id="75abb-116">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="75abb-117">Nainstalujte modul runtime .NET Core na serveru.</span><span class="sxs-lookup"><span data-stu-id="75abb-117">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="75abb-118">Přejděte [.NET Core všechny soubory ke stažení stránky](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="75abb-118">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="75abb-119">Vyberte nejnovější modul runtime – ve verzi preview ze seznamu **Runtime**.</span><span class="sxs-lookup"><span data-stu-id="75abb-119">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="75abb-120">Vyberte a postupujte podle pokynů pro Ubuntu, které odpovídají verze Ubuntu serveru.</span><span class="sxs-lookup"><span data-stu-id="75abb-120">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="75abb-121">Stávající aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75abb-121">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="75abb-122">Publikování a zkopírujte myší na aplikaci</span><span class="sxs-lookup"><span data-stu-id="75abb-122">Publish and copy over the app</span></span>

<span data-ttu-id="75abb-123">Konfigurace aplikace pro [nasazení závisí na architektuře](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="75abb-123">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="75abb-124">Spustit [dotnet publikovat](/dotnet/core/tools/dotnet-publish) z vývojového prostředí pro balíček aplikace do adresáře (například *bin/Release/&lt;target_framework_moniker&gt;/ publish*), který můžete Spusťte na serveru:</span><span class="sxs-lookup"><span data-stu-id="75abb-124">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="75abb-125">Aplikace můžete také publikovat jako [samostatná nasazení](/dotnet/core/deploying/#self-contained-deployments-scd) Pokud nechcete zachovat modulu runtime .NET Core na serveru.</span><span class="sxs-lookup"><span data-stu-id="75abb-125">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="75abb-126">Aplikace ASP.NET Core zkopírujte na server pomocí nástroje, které se integruje do pracovního postupu organizace (třeba spojovací bod služby, SFTP).</span><span class="sxs-lookup"><span data-stu-id="75abb-126">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="75abb-127">Je běžné vyhledejte webové aplikace v rámci *var* adresář (třeba *aspnetcore/var/hellomvc*).</span><span class="sxs-lookup"><span data-stu-id="75abb-127">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="75abb-128">V případě produkčního nasazení pracovního postupu průběžné integrace funguje publikování aplikace a kopírování prostředky na server.</span><span class="sxs-lookup"><span data-stu-id="75abb-128">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="75abb-129">Testování aplikace:</span><span class="sxs-lookup"><span data-stu-id="75abb-129">Test the app:</span></span>

1. <span data-ttu-id="75abb-130">Z příkazového řádku, spusťte aplikaci: `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="75abb-130">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="75abb-131">V prohlížeči přejděte na `http://<serveraddress>:<port>` k ověření, že aplikace funguje na platformě Linux místně.</span><span class="sxs-lookup"><span data-stu-id="75abb-131">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="75abb-132">Konfigurace reverzního proxy serveru</span><span class="sxs-lookup"><span data-stu-id="75abb-132">Configure a reverse proxy server</span></span>

<span data-ttu-id="75abb-133">Reverzní proxy server je společné nastavení pro poskytování dynamické webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="75abb-133">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="75abb-134">Reverzní proxy server ukončí požadavek HTTP a předá jej do aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75abb-134">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="75abb-135">Buď konfiguraci&mdash;s nebo bez něj reverzní proxy server&mdash;je platný a podporované konfigurace pro hostování pro ASP.NET Core 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="75abb-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="75abb-136">Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="75abb-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="75abb-137">Použít reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="75abb-137">Use a reverse proxy server</span></span>

<span data-ttu-id="75abb-138">Kestrel se skvěle hodí pro poskytování dynamický obsah z ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75abb-138">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="75abb-139">Však nejsou možnosti obsluhující web jako komplexní jako servery služby IIS, Apache nebo Nginx.</span><span class="sxs-lookup"><span data-stu-id="75abb-139">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="75abb-140">Reverzní proxy server může převzít práce, jako je například obsluhuje statický obsah, ukládání do mezipaměti požadavky, komprese požadavků a ukončení protokolu SSL ze serveru HTTP.</span><span class="sxs-lookup"><span data-stu-id="75abb-140">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="75abb-141">Reverzní proxy server může nacházet na vyhrazený počítač nebo může být nasadí společně se službou serveru HTTP.</span><span class="sxs-lookup"><span data-stu-id="75abb-141">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="75abb-142">Pro účely tohoto průvodce se používá jednu instanci serveru Nginx.</span><span class="sxs-lookup"><span data-stu-id="75abb-142">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="75abb-143">Běží na stejném serveru, spolu s HTTP serverem.</span><span class="sxs-lookup"><span data-stu-id="75abb-143">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="75abb-144">Na základě požadavků, může být zvolen jiný instalační program.</span><span class="sxs-lookup"><span data-stu-id="75abb-144">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="75abb-145">Vzhledem k tomu, že žádosti jsou předávány podle reverzní proxy server, použít [předané Middleware záhlaví](xref:host-and-deploy/proxy-load-balancer) z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="75abb-145">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="75abb-146">Middleware aktualizace `Request.Scheme`, použije `X-Forwarded-Proto` záhlaví tak, že identifikátory URI pro přesměrování a další zásady zabezpečení pracovat správně.</span><span class="sxs-lookup"><span data-stu-id="75abb-146">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="75abb-147">Jakékoli součásti, která závisí na schéma, jako je například ověřování, generování odkazů, přesměrování a zeměpisná poloha, musí být umístěn po vyvolání Middleware předané záhlaví.</span><span class="sxs-lookup"><span data-stu-id="75abb-147">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="75abb-148">Jako obecné pravidlo by měla předávat Middleware záhlaví spustit před dalším middlewarem s výjimkou diagnostiky a middleware pro zpracování chyb.</span><span class="sxs-lookup"><span data-stu-id="75abb-148">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="75abb-149">Toto uspořádání zajistí, že middleware spoléhání se na informace předávané záhlaví může spotřebovat hodnoty hlavičky pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="75abb-149">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="75abb-150">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="75abb-150">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="75abb-151">Vyvolat [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metoda `Startup.Configure` před voláním [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) nebo podobné režimu middleware ověřování.</span><span class="sxs-lookup"><span data-stu-id="75abb-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="75abb-152">Nakonfigurujte middleware předávat `X-Forwarded-For` a `X-Forwarded-Proto` hlavičky:</span><span class="sxs-lookup"><span data-stu-id="75abb-152">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="75abb-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="75abb-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="75abb-154">Vyvolat [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metoda `Startup.Configure` před voláním [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) a [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) nebo podobné schéma ověřování middleware.</span><span class="sxs-lookup"><span data-stu-id="75abb-154">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="75abb-155">Nakonfigurujte middleware předávat `X-Forwarded-For` a `X-Forwarded-Proto` hlavičky:</span><span class="sxs-lookup"><span data-stu-id="75abb-155">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="75abb-156">Pokud ne [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) jsou určené pro middleware, jsou výchozí hlavičky pro předávání `None`.</span><span class="sxs-lookup"><span data-stu-id="75abb-156">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="75abb-157">Další konfigurace může být nezbytný pro aplikací hostovaných za službou proxy servery a nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="75abb-157">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="75abb-158">Další informace najdete v tématu [konfigurace ASP.NET Core práci se servery proxy a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="75abb-158">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="75abb-159">Instalaci serveru Nginx</span><span class="sxs-lookup"><span data-stu-id="75abb-159">Install Nginx</span></span>

<span data-ttu-id="75abb-160">Použití `apt-get` nainstaluje server Nginx.</span><span class="sxs-lookup"><span data-stu-id="75abb-160">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="75abb-161">Instalační program vytvoří *systemd* init skript, který spouští server Nginx jako démon na spuštění systému.</span><span class="sxs-lookup"><span data-stu-id="75abb-161">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> 

```bash
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update
apt-get install nginx
```

<span data-ttu-id="75abb-162">Archiv služby Ubuntu osobní balíčků (PPA) udržuje dobrovolným a není distribuuje společnost [nginx.org](https://nginx.org/). Další informace najdete v tématu [Nginx: binární verze: balíčky oficiální Debian nebo Ubuntu](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span><span class="sxs-lookup"><span data-stu-id="75abb-162">The Ubuntu Personal Package Archive (PPA) is maintained by volunteers and isn't distributed by [nginx.org](https://nginx.org/). For more information, see [Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="75abb-163">Pokud volitelný moduly Nginx, může být potřeba vytváření Nginx ze zdroje.</span><span class="sxs-lookup"><span data-stu-id="75abb-163">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="75abb-164">Protože server Nginx nainstaloval poprvé, explicitně spusťte ji spuštěním:</span><span class="sxs-lookup"><span data-stu-id="75abb-164">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="75abb-165">Ověřte, že prohlížeč zobrazí výchozí úvodní stránka pro server Nginx.</span><span class="sxs-lookup"><span data-stu-id="75abb-165">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="75abb-166">Cílová stránka je dostupný na `http://<server_IP_address>/index.nginx-debian.html`.</span><span class="sxs-lookup"><span data-stu-id="75abb-166">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="75abb-167">Konfigurace serveru Nginx</span><span class="sxs-lookup"><span data-stu-id="75abb-167">Configure Nginx</span></span>

<span data-ttu-id="75abb-168">Chcete-li nakonfigurovat Nginx jako reverzní proxy server pro směrování požadavků do vaší aplikace ASP.NET Core, upravte */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="75abb-168">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="75abb-169">Otevřete v textovém editoru a nahraďte obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="75abb-169">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

<span data-ttu-id="75abb-170">Pokud ne `server_name` shody, Nginx používá výchozí server.</span><span class="sxs-lookup"><span data-stu-id="75abb-170">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="75abb-171">Pokud není definovaný žádný výchozí server, první server v konfiguračním souboru je výchozí server.</span><span class="sxs-lookup"><span data-stu-id="75abb-171">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="75abb-172">Jako osvědčený postup přidáte určité výchozí server, který vrátí stavový kód 444 v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="75abb-172">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="75abb-173">Příklad konfigurace serveru výchozí je:</span><span class="sxs-lookup"><span data-stu-id="75abb-173">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="75abb-174">S předchozím konfigurační soubor a výchozí server, server Nginx přijímá veřejné provozu na portu 80 se hlavička hostitele `example.com` nebo `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="75abb-174">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="75abb-175">Požadavky nejsou odpovídající tito hostitelé se získat předány Kestrel.</span><span class="sxs-lookup"><span data-stu-id="75abb-175">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="75abb-176">Nginx předává požadavky odpovídající Kestrel na `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="75abb-176">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="75abb-177">Zobrazit [jak nginx zpracovává žádost](https://nginx.org/docs/http/request_processing.html) Další informace.</span><span class="sxs-lookup"><span data-stu-id="75abb-177">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span> <span data-ttu-id="75abb-178">Chcete-li změnit Kestrel jeho IP adresa/port, [Kestrel: konfigurace koncového bodu](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="75abb-178">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="75abb-179">Nepodařilo se určit správnou [název_serveru směrnice](https://nginx.org/docs/http/server_names.html) zpřístupňuje aplikaci tak, aby slabá místa zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="75abb-179">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="75abb-180">Vazby zástupný znak subdoménu (například `*.example.com`) nemá představovat toto bezpečnostní riziko, pokud řídíte celý nadřazené domény (nikoli `*.com`, což je ohrožené).</span><span class="sxs-lookup"><span data-stu-id="75abb-180">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="75abb-181">Zobrazit [rfc7230 části-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Další informace.</span><span class="sxs-lookup"><span data-stu-id="75abb-181">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="75abb-182">Jakmile se naváže v konfiguraci serveru Nginx, spusťte `sudo nginx -t` syntaxi konfiguračních souborů.</span><span class="sxs-lookup"><span data-stu-id="75abb-182">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="75abb-183">Pokud konfigurační soubor test je úspěšný, vynutit Nginx tak, aby získaly změn spuštěním `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="75abb-183">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="75abb-184">Pro přímé spouštění aplikace na serveru:</span><span class="sxs-lookup"><span data-stu-id="75abb-184">To directly run the app on the server:</span></span>

1. <span data-ttu-id="75abb-185">Přejděte do adresáře aplikace.</span><span class="sxs-lookup"><span data-stu-id="75abb-185">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="75abb-186">Spuštění spustitelného souboru aplikace: `./<app_executable>`.</span><span class="sxs-lookup"><span data-stu-id="75abb-186">Run the app's executable: `./<app_executable>`.</span></span>

<span data-ttu-id="75abb-187">Pokud dojde k chybě oprávnění, změňte oprávnění:</span><span class="sxs-lookup"><span data-stu-id="75abb-187">If a permissions error occurs, change the permissions:</span></span>

```console
chmod u+x <app_executable>
```

<span data-ttu-id="75abb-188">Pokud aplikace běží na serveru, ale přestane reagovat přes Internet, zkontrolujte bránu firewall serveru a potvrďte, že je otevřený port 80.</span><span class="sxs-lookup"><span data-stu-id="75abb-188">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="75abb-189">Pokud používáte virtuální počítač Azure s Ubuntu, přidejte pravidlo skupiny zabezpečení sítě (NSG), která umožní příchozí port 80 provoz.</span><span class="sxs-lookup"><span data-stu-id="75abb-189">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="75abb-190">Není nutné povolit pravidlo odchozí port 80 jako odchozí provoz se automaticky udělí, když je povolený příchozí pravidlo.</span><span class="sxs-lookup"><span data-stu-id="75abb-190">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="75abb-191">Po dokončení testování aplikace vypnout aplikaci s `Ctrl+C` příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="75abb-191">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="75abb-192">Monitorování aplikace</span><span class="sxs-lookup"><span data-stu-id="75abb-192">Monitoring the app</span></span>

<span data-ttu-id="75abb-193">Server je instalačního programu předat požadavky na `http://<serveraddress>:80` k aplikaci ASP.NET Core spuštěnou v Kestrel na `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="75abb-193">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="75abb-194">Server Nginx se ale nastavit ke správě procesu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="75abb-194">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="75abb-195">*systemd* slouží k vytvoření souboru služby ke spuštění a monitorování základní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="75abb-195">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="75abb-196">*systemd* je init systém, který poskytuje řadu výkonných funkcí pro spouštění, zastavování a Správa procesů.</span><span class="sxs-lookup"><span data-stu-id="75abb-196">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="75abb-197">Vytvoření souboru služby</span><span class="sxs-lookup"><span data-stu-id="75abb-197">Create the service file</span></span>

<span data-ttu-id="75abb-198">Vytvoření definičního souboru služby:</span><span class="sxs-lookup"><span data-stu-id="75abb-198">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="75abb-199">Následuje příklad souboru služby pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="75abb-199">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="75abb-200">Pokud uživatel *www-data* nepoužívá konfigurace, musí nejprve vytvořit uživatelem definované tady a pro soubory zadané správné vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="75abb-200">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="75abb-201">Linux má systém souborů s rozlišením velkých.</span><span class="sxs-lookup"><span data-stu-id="75abb-201">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="75abb-202">Nastavení ASPNETCORE_ENVIRONMENT "Produkční" výsledky hledání pro konfigurační soubor *appsettings. Production.JSON*, nikoli *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="75abb-202">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="75abb-203">Některé hodnoty (například připojovací řetězce SQL) musí být uvozena pro zprostředkovatele konfigurace pro čtení proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="75abb-203">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="75abb-204">Použijte následující příkaz k vygenerování správně uvozený uvozovacím znakem hodnoty pro použití v konfiguračním souboru:</span><span class="sxs-lookup"><span data-stu-id="75abb-204">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="75abb-205">Uložte soubor a povolení služby.</span><span class="sxs-lookup"><span data-stu-id="75abb-205">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="75abb-206">Spusťte službu a ověřte, zda je spuštěna.</span><span class="sxs-lookup"><span data-stu-id="75abb-206">Start the service and verify that it's running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="75abb-207">Reverzní proxy server nakonfigurovat a spravovat prostřednictvím systemd Kestrel, webové aplikace plně konfigurována a je přístupný z prohlížeče na místním počítači v `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="75abb-207">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="75abb-208">Je také přístupné ze vzdáleného počítače, blokování jakoukoli jinou bránu firewall, která může blokovat.</span><span class="sxs-lookup"><span data-stu-id="75abb-208">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="75abb-209">Kontrola hlavičky odpovědi `Server` záhlaví zobrazuje obsluhuje Kestrel aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75abb-209">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="75abb-210">Zobrazení protokolů</span><span class="sxs-lookup"><span data-stu-id="75abb-210">Viewing logs</span></span>

<span data-ttu-id="75abb-211">Od webové aplikace pomocí Kestrel se spravuje pomocí `systemd`, centralizované deníku se protokolují všechny události a procesy.</span><span class="sxs-lookup"><span data-stu-id="75abb-211">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="75abb-212">Ale tento deník obsahuje všechny položky pro všechny služby a spravuje procesy `systemd`.</span><span class="sxs-lookup"><span data-stu-id="75abb-212">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="75abb-213">Chcete-li zobrazit `kestrel-hellomvc.service`-konkrétní položky, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="75abb-213">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="75abb-214">Pro další filtrování, možnosti, jak `--since today`, `--until 1 hour ago` nebo kombinaci těchto způsobů můžete omezit množství vrácených záznamů.</span><span class="sxs-lookup"><span data-stu-id="75abb-214">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="75abb-215">Zabezpečení aplikace</span><span class="sxs-lookup"><span data-stu-id="75abb-215">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="75abb-216">Povolit AppArmor</span><span class="sxs-lookup"><span data-stu-id="75abb-216">Enable AppArmor</span></span>

<span data-ttu-id="75abb-217">Linux zabezpečení moduly (LSM) je architektura, která je součástí linuxového jádra od Linux 2.6.</span><span class="sxs-lookup"><span data-stu-id="75abb-217">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="75abb-218">LSM podporuje různé implementace modulů zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="75abb-218">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="75abb-219">[AppArmor](https://wiki.ubuntu.com/AppArmor) je LSM, která implementuje systém povinné řízení přístupu, který umožňuje uzavírání program omezenou sadu prostředků.</span><span class="sxs-lookup"><span data-stu-id="75abb-219">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="75abb-220">Zajištění AppArmor je povolená a správně nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="75abb-220">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="75abb-221">Konfigurace brány firewall</span><span class="sxs-lookup"><span data-stu-id="75abb-221">Configuring the firewall</span></span>

<span data-ttu-id="75abb-222">Zavřete vypnout všechny externí porty, které nejsou používány.</span><span class="sxs-lookup"><span data-stu-id="75abb-222">Close off all external ports that are not in use.</span></span> <span data-ttu-id="75abb-223">Znamená přístupnější aplikaci brány firewall (ufw) poskytuje front-endu pro `iptables` tím, že poskytuje rozhraní příkazového řádku pro konfiguraci brány firewall.</span><span class="sxs-lookup"><span data-stu-id="75abb-223">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="75abb-224">Ověřte, že `ufw` nakonfigurována, aby umožňovala provoz na všech portech potřeba.</span><span class="sxs-lookup"><span data-stu-id="75abb-224">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="75abb-225">Zabezpečení serveru Nginx</span><span class="sxs-lookup"><span data-stu-id="75abb-225">Securing Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="75abb-226">Změnit název odpovědi serveru Nginx</span><span class="sxs-lookup"><span data-stu-id="75abb-226">Change the Nginx response name</span></span>

<span data-ttu-id="75abb-227">Edit *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="75abb-227">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="75abb-228">Konfigurace možností</span><span class="sxs-lookup"><span data-stu-id="75abb-228">Configure options</span></span>

<span data-ttu-id="75abb-229">Konfigurace serveru s další požadované moduly.</span><span class="sxs-lookup"><span data-stu-id="75abb-229">Configure the server with additional required modules.</span></span> <span data-ttu-id="75abb-230">Zvažte použití brány firewall webových aplikací, jako například [ModSecurity](https://www.modsecurity.org/), Posilte zabezpečení aplikace.</span><span class="sxs-lookup"><span data-stu-id="75abb-230">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="configure-ssl"></a><span data-ttu-id="75abb-231">Konfigurace SSL</span><span class="sxs-lookup"><span data-stu-id="75abb-231">Configure SSL</span></span>

* <span data-ttu-id="75abb-232">Konfigurace serveru tak, aby naslouchala na přenosy HTTPS na portu `443` zadáním platný certifikát vydaný důvěryhodného certifikátu autority (CA).</span><span class="sxs-lookup"><span data-stu-id="75abb-232">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="75abb-233">Posílení zabezpečení, když některé postupy uvedené v následující */etc/nginx/nginx.conf* souboru.</span><span class="sxs-lookup"><span data-stu-id="75abb-233">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="75abb-234">Mezi příklady patří výběrem silnější šifrování a přesměrovat veškerý provoz prostřednictvím protokolu HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="75abb-234">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="75abb-235">Přidání `HTTP Strict-Transport-Security` záhlaví (HSTS) zajišťuje, všechny následné požadavky odeslané klientem jsou pouze přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="75abb-235">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="75abb-236">Nepřidávejte záhlaví zabezpečení přenosu Strict nebo zvolit odpovídající `max-age` Pokud bude v budoucnu zakázání protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="75abb-236">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="75abb-237">Přidat */etc/nginx/proxy.conf* konfiguračního souboru:</span><span class="sxs-lookup"><span data-stu-id="75abb-237">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="75abb-238">Upravit */etc/nginx/nginx.conf* konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="75abb-238">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="75abb-239">Tento příklad obsahuje oba `http` a `server` oddílů v souboru jednu konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="75abb-239">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="75abb-240">Zabezpečení serveru Nginx z útoků typu clickjacking</span><span class="sxs-lookup"><span data-stu-id="75abb-240">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="75abb-241">Útoků typu Clickjacking je škodlivý techniku, která umožňuje shromažďovat nakažené uživatel klikne.</span><span class="sxs-lookup"><span data-stu-id="75abb-241">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="75abb-242">Útoků typu Clickjacking triky victim (návštěvníka) do kliknutím na nakažené lokality.</span><span class="sxs-lookup"><span data-stu-id="75abb-242">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="75abb-243">Použití X-FRAME-OPTIONS pro zabezpečení webu.</span><span class="sxs-lookup"><span data-stu-id="75abb-243">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="75abb-244">Upravit *nginx.conf* souboru:</span><span class="sxs-lookup"><span data-stu-id="75abb-244">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="75abb-245">Přidejte řádek `add_header X-Frame-Options "SAMEORIGIN";` a uložte soubor a pak restartujte server Nginx.</span><span class="sxs-lookup"><span data-stu-id="75abb-245">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="75abb-246">Typ MIME pro analýzu sítě</span><span class="sxs-lookup"><span data-stu-id="75abb-246">MIME-type sniffing</span></span>

<span data-ttu-id="75abb-247">Tato hlavička brání většina prohlížečů z MIME pro analýzu sítě odpověď od deklarovaný typ obsahu, jako hlavičku dostane pokyn, abyste nepřepsali typ obsahu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="75abb-247">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="75abb-248">S `nosniff` možnost, pokud server říká, že obsah je "text/html", v prohlížeči se vykreslí jako "text/html".</span><span class="sxs-lookup"><span data-stu-id="75abb-248">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="75abb-249">Upravit *nginx.conf* souboru:</span><span class="sxs-lookup"><span data-stu-id="75abb-249">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="75abb-250">Přidejte řádek `add_header X-Content-Type-Options "nosniff";` a uložte soubor a pak restartujte server Nginx.</span><span class="sxs-lookup"><span data-stu-id="75abb-250">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75abb-251">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="75abb-251">Additional resources</span></span>

* [<span data-ttu-id="75abb-252">Serveru Nginx: Binární verze: balíčky oficiální Debian nebo Ubuntu</span><span class="sxs-lookup"><span data-stu-id="75abb-252">Nginx: Binary Releases: Official Debian/Ubuntu packages</span></span>](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* [<span data-ttu-id="75abb-253">Konfigurace ASP.NET Core práci se servery proxy a nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="75abb-253">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="75abb-254">NGINX: Předané záhlaví pomocí</span><span class="sxs-lookup"><span data-stu-id="75abb-254">NGINX: Using the Forwarded header</span></span>](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
