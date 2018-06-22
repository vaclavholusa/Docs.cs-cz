---
title: Hostování v systému Linux s Nginx ASP.NET Core
author: rick-anderson
description: Zjistěte, jak nastavit jako reverzní proxy server na Ubuntu 16.04 pro přenos dat protokolu HTTP do webové aplikace ASP.NET Core systémem Kestrel Nginx.
ms.author: riande
ms.custom: mvc
ms.date: 05/22/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 374b13e0851cd171a7d8500a4965851a3a0eb49c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277371"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="56aa4-103">Hostování v systému Linux s Nginx ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56aa4-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="56aa4-104">Podle [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="56aa4-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="56aa4-105">Tato příručka vysvětluje nastavení prostředí ASP.NET Core produkční prostředí na Ubuntu 16.04 server.</span><span class="sxs-lookup"><span data-stu-id="56aa4-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="56aa4-106">Tyto pokyny pravděpodobně pracovat s novější verze Ubuntu, ale pokynů nebyly testovány s novější verzí.</span><span class="sxs-lookup"><span data-stu-id="56aa4-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

> [!NOTE]
> <span data-ttu-id="56aa4-107">Pro Ubuntu 14.04 *supervisord* se doporučuje jako řešení pro sledování, zpracovávají Kestrel.</span><span class="sxs-lookup"><span data-stu-id="56aa4-107">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="56aa4-108">*systemd* na Ubuntu 14.04 není dostupný.</span><span class="sxs-lookup"><span data-stu-id="56aa4-108">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="56aa4-109">Ubuntu 14.04 pokyny najdete v tématu [předchozí verzi tohoto tématu](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="56aa4-109">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="56aa4-110">Tato příručka:</span><span class="sxs-lookup"><span data-stu-id="56aa4-110">This guide:</span></span>

* <span data-ttu-id="56aa4-111">Umístí stávající aplikace ASP.NET Core za reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="56aa4-111">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="56aa4-112">Nastaví reverzní proxy server pro předávání požadavků na Kestrel webový server.</span><span class="sxs-lookup"><span data-stu-id="56aa4-112">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="56aa4-113">Zajišťuje, že webová aplikace spuštěna při spuštění jako démon.</span><span class="sxs-lookup"><span data-stu-id="56aa4-113">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="56aa4-114">Konfiguruje nástroj pro správu procesu pomohou restartování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="56aa4-114">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56aa4-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="56aa4-115">Prerequisites</span></span>

1. <span data-ttu-id="56aa4-116">Přístup k serveru Ubuntu 16.04 s standardní uživatelský účet s oprávněním sudo.</span><span class="sxs-lookup"><span data-stu-id="56aa4-116">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="56aa4-117">Nainstalujte na .NET Core runtime na serveru.</span><span class="sxs-lookup"><span data-stu-id="56aa4-117">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="56aa4-118">Přejděte [.NET Core všechny soubory ke stažení stránky](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="56aa4-118">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="56aa4-119">Vyberte ze seznamu v části nejnovější modul runtime bez preview **Runtime**.</span><span class="sxs-lookup"><span data-stu-id="56aa4-119">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="56aa4-120">Vyberte a postupujte podle pokynů pro Ubuntu, které odpovídají verzi Ubuntu server.</span><span class="sxs-lookup"><span data-stu-id="56aa4-120">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="56aa4-121">Stávající aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="56aa4-121">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="56aa4-122">Publikování a zkopírujte přes aplikace</span><span class="sxs-lookup"><span data-stu-id="56aa4-122">Publish and copy over the app</span></span>

<span data-ttu-id="56aa4-123">Konfigurace aplikace pro [nasazení závislé na framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="56aa4-123">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="56aa4-124">Spustit [dotnet publikování](/dotnet/core/tools/dotnet-publish) z vývojového prostředí pro zabalení aplikace do adresáře (například *Koš a verze nebo&lt;target_framework_moniker&gt;/ publikovat*), můžete Spusťte na serveru:</span><span class="sxs-lookup"><span data-stu-id="56aa4-124">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="56aa4-125">Aplikace lze také publikovat jako [samostatná nasazení](/dotnet/core/deploying/#self-contained-deployments-scd) Pokud nechcete udržovat na .NET Core runtime na serveru.</span><span class="sxs-lookup"><span data-stu-id="56aa4-125">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="56aa4-126">Zkopírujte aplikace ASP.NET Core k serveru pomocí nástroje, který se integruje do pracovního postupu organizace (například spojovací bod služby, pomocí protokolu SFTP).</span><span class="sxs-lookup"><span data-stu-id="56aa4-126">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="56aa4-127">Je běžné najít webové aplikace v rámci *var* directory (například *aspnetcore/var/hellomvc*).</span><span class="sxs-lookup"><span data-stu-id="56aa4-127">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="56aa4-128">V části nasazení produkční scénář průběžnou integraci pracovní postup funguje publikování aplikace a kopírování prostředků na server.</span><span class="sxs-lookup"><span data-stu-id="56aa4-128">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="56aa4-129">Testovací aplikace:</span><span class="sxs-lookup"><span data-stu-id="56aa4-129">Test the app:</span></span>

1. <span data-ttu-id="56aa4-130">Z příkazového řádku, spusťte aplikaci: `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="56aa4-130">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="56aa4-131">V prohlížeči přejděte na `http://<serveraddress>:<port>` k ověření, že aplikace funguje v systému Linux místně.</span><span class="sxs-lookup"><span data-stu-id="56aa4-131">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="56aa4-132">Konfigurace serveru reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="56aa4-132">Configure a reverse proxy server</span></span>

<span data-ttu-id="56aa4-133">Reverzní proxy server je běžné instalační program pro obsluhující dynamické webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="56aa4-133">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="56aa4-134">Reverzní proxy server ukončí požadavek HTTP a předává do aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="56aa4-134">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="56aa4-135">Buď konfiguraci&mdash;s nebo bez reverzní proxy server&mdash;je platný a podporované konfigurace hostování pro technologii ASP.NET Core 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="56aa4-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="56aa4-136">Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="56aa4-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="56aa4-137">Použít reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="56aa4-137">Use a reverse proxy server</span></span>

<span data-ttu-id="56aa4-138">Kestrel je skvělá pro obsluhující dynamický obsah z ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="56aa4-138">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="56aa4-139">Však nejsou webové funkce slouží jako bohaté funkce jako servery, například služby IIS, Apache nebo Nginx.</span><span class="sxs-lookup"><span data-stu-id="56aa4-139">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="56aa4-140">Reverzní proxy server můžete přesměrovat pracovní například statický obsah obsluhuje, ukládání do mezipaměti požadavky, komprese požadavků a ukončení protokolu SSL ze serveru HTTP.</span><span class="sxs-lookup"><span data-stu-id="56aa4-140">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="56aa4-141">Reverzní proxy server může být na vyhrazeném počítači nebo může být nasazeny společně se HTTP server.</span><span class="sxs-lookup"><span data-stu-id="56aa4-141">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="56aa4-142">Pro účely tohoto průvodce se používá jednu instanci Nginx.</span><span class="sxs-lookup"><span data-stu-id="56aa4-142">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="56aa4-143">Běží na stejném serveru, spolu s HTTP server.</span><span class="sxs-lookup"><span data-stu-id="56aa4-143">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="56aa4-144">Na základě požadavků, může být zvolen jiné instalace.</span><span class="sxs-lookup"><span data-stu-id="56aa4-144">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="56aa4-145">Protože požadavky jsou předávány podle reverzní proxy server, použijte [předané Middleware hlavičky](xref:host-and-deploy/proxy-load-balancer) z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="56aa4-145">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="56aa4-146">Middleware aktualizace `Request.Scheme`pomocí `X-Forwarded-Proto` záhlaví, tak, že přesměrování identifikátory URI a jiné zásady zabezpečení pracovat správně.</span><span class="sxs-lookup"><span data-stu-id="56aa4-146">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="56aa4-147">Všechny součásti, které závisí na schéma, jako je například ověřování, generování odkazů, přesměrování a informace o zeměpisné poloze, musí být umístěny po vyvolání hlavičky Middleware předávat.</span><span class="sxs-lookup"><span data-stu-id="56aa4-147">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="56aa4-148">Obecně platí by měla předávat Middleware hlavičky spustit před další middleware s výjimkou diagnostiky a zpracování middleware chyb.</span><span class="sxs-lookup"><span data-stu-id="56aa4-148">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="56aa4-149">Toto uspořádání zajistí, že middleware spoléhat na informace předávané hlavičky spotřebovat hodnoty hlavičky pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="56aa4-149">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="56aa4-150">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="56aa4-150">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="56aa4-151">Vyvolání [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metoda v `Startup.Configure` před voláním [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) nebo podobné middleware ověřování schématu.</span><span class="sxs-lookup"><span data-stu-id="56aa4-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="56aa4-152">Nakonfigurujte middleware předávat `X-Forwarded-For` a `X-Forwarded-Proto` hlavičky:</span><span class="sxs-lookup"><span data-stu-id="56aa4-152">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="56aa4-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="56aa4-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="56aa4-154">Vyvolání [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metoda v `Startup.Configure` před voláním [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) a [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) nebo podobné schéma ověřování middleware.</span><span class="sxs-lookup"><span data-stu-id="56aa4-154">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="56aa4-155">Nakonfigurujte middleware předávat `X-Forwarded-For` a `X-Forwarded-Proto` hlavičky:</span><span class="sxs-lookup"><span data-stu-id="56aa4-155">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="56aa4-156">Pokud žádné [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) jsou určené pro middleware, jsou výchozí hlavičky předávat `None`.</span><span class="sxs-lookup"><span data-stu-id="56aa4-156">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="56aa4-157">Další konfigurace může být potřeba pro aplikace, které jsou hostovány za proxy servery a nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="56aa4-157">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="56aa4-158">Další informace najdete v tématu [konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="56aa4-158">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="56aa4-159">Nainstalujte Nginx</span><span class="sxs-lookup"><span data-stu-id="56aa4-159">Install Nginx</span></span>

<span data-ttu-id="56aa4-160">Použití `apt-get` k instalaci Nginx.</span><span class="sxs-lookup"><span data-stu-id="56aa4-160">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="56aa4-161">Instalační program vytvoří *systemd* init skript, který spouští Nginx jako démon na spuštění systému.</span><span class="sxs-lookup"><span data-stu-id="56aa4-161">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> 

```bash
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update
apt-get install nginx
```

<span data-ttu-id="56aa4-162">Ubuntu osobní balíček archivu (zásad pro posuzování projektů) je možný díky dobrovolníků a není distribuovat [nginx.org](https://nginx.org/). Další informace najdete v tématu [Nginx: binární verze: balíčky oficiální Debian/Ubuntu](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span><span class="sxs-lookup"><span data-stu-id="56aa4-162">The Ubuntu Personal Package Archive (PPA) is maintained by volunteers and isn't distributed by [nginx.org](https://nginx.org/). For more information, see [Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="56aa4-163">Pokud volitelné moduly Nginx, může být potřeba vytváření Nginx ze zdroje.</span><span class="sxs-lookup"><span data-stu-id="56aa4-163">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="56aa4-164">Vzhledem k tomu, že Nginx byla nainstalována poprvé, explicitně spusťte ji spuštěním:</span><span class="sxs-lookup"><span data-stu-id="56aa4-164">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="56aa4-165">Ověřte, zda že prohlížeč zobrazí výchozí úvodní stránka pro Nginx.</span><span class="sxs-lookup"><span data-stu-id="56aa4-165">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="56aa4-166">Cílová stránka je dostupný v `http://<server_IP_address>/index.nginx-debian.html`.</span><span class="sxs-lookup"><span data-stu-id="56aa4-166">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="56aa4-167">Konfigurace Nginx</span><span class="sxs-lookup"><span data-stu-id="56aa4-167">Configure Nginx</span></span>

<span data-ttu-id="56aa4-168">Pokud chcete konfigurovat Nginx jako reverzní proxy server pro směrování požadavků do vaší aplikace ASP.NET Core, upravte */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="56aa4-168">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="56aa4-169">Otevřete v textovém editoru a nahraďte jeho obsah následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="56aa4-169">Open it in a text editor, and replace the contents with the following:</span></span>

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

<span data-ttu-id="56aa4-170">Pokud ne `server_name` odpovídá Nginx používá výchozí server.</span><span class="sxs-lookup"><span data-stu-id="56aa4-170">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="56aa4-171">Pokud je definována žádná výchozí server, první server v konfiguračním souboru je výchozí server.</span><span class="sxs-lookup"><span data-stu-id="56aa4-171">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="56aa4-172">Jako osvědčený postup přidejte konkrétní výchozí server, který vrátí stavový kód 444 v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="56aa4-172">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="56aa4-173">Příklad konfigurace serveru výchozí je:</span><span class="sxs-lookup"><span data-stu-id="56aa4-173">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="56aa4-174">S předchozím konfigurační soubor a výchozí server, Nginx přijímá veřejné přenosy na portu 80 s Hlavička hostitele `example.com` nebo `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="56aa4-174">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="56aa4-175">Požadavky není odpovídající tito hostitelé nebude získat předávat Kestrel.</span><span class="sxs-lookup"><span data-stu-id="56aa4-175">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="56aa4-176">Nginx předává požadavky odpovídající Kestrel v `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="56aa4-176">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="56aa4-177">V tématu [jak nginx zpracovává žádost o](https://nginx.org/docs/http/request_processing.html) Další informace.</span><span class="sxs-lookup"><span data-stu-id="56aa4-177">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="56aa4-178">Nepodařilo se určit správný [název_serveru direktiva](https://nginx.org/docs/http/server_names.html) zpřístupní v aplikaci ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="56aa4-178">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="56aa4-179">Vazba subdomény zástupný znak (například `*.example.com`) nemá představovat toto bezpečnostní riziko, pokud řízení celého nadřazené domény (Naproti tomu `*.com`, což je snadno napadnutelný).</span><span class="sxs-lookup"><span data-stu-id="56aa4-179">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="56aa4-180">V tématu [rfc7230 části-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Další informace.</span><span class="sxs-lookup"><span data-stu-id="56aa4-180">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="56aa4-181">Po vytvoření konfigurace Nginx spustit `sudo nginx -t` syntaxi konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="56aa4-181">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="56aa4-182">Pokud se test souboru konfigurace je úspěšné, vynutit Nginx mohla vybrat změny spuštěním `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="56aa4-182">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="56aa4-183">Pro přímé spouštění aplikace na serveru:</span><span class="sxs-lookup"><span data-stu-id="56aa4-183">To directly run the app on the server:</span></span>

1. <span data-ttu-id="56aa4-184">Přejděte do adresáře aplikace.</span><span class="sxs-lookup"><span data-stu-id="56aa4-184">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="56aa4-185">Spustit spustitelný soubor aplikace: `./<app_executable>`.</span><span class="sxs-lookup"><span data-stu-id="56aa4-185">Run the app's executable: `./<app_executable>`.</span></span>

<span data-ttu-id="56aa4-186">Pokud dojde k chybě oprávnění ke změně oprávnění:</span><span class="sxs-lookup"><span data-stu-id="56aa4-186">If a permissions error occurs, change the permissions:</span></span>

```console
chmod u+x <app_executable>
```

<span data-ttu-id="56aa4-187">Pokud aplikace běží na serveru, ale přestane reagovat přes Internet, zkontrolujte brány firewall serveru a ověřte, zda je otevřený port 80.</span><span class="sxs-lookup"><span data-stu-id="56aa4-187">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="56aa4-188">Pokud používáte virtuálního počítače s Ubuntu Azure, přidejte pravidlo skupina zabezpečení sítě (NSG), které umožní příchozí port 80 komunikaci.</span><span class="sxs-lookup"><span data-stu-id="56aa4-188">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="56aa4-189">Jako odchozí provoz je automaticky přiděleno, pokud je povolena příchozí pravidlo, není třeba povolit pravidlo odchozí port 80.</span><span class="sxs-lookup"><span data-stu-id="56aa4-189">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="56aa4-190">Po dokončení testování aplikace vypněte aplikace s `Ctrl+C` na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="56aa4-190">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="56aa4-191">Monitorování aplikace</span><span class="sxs-lookup"><span data-stu-id="56aa4-191">Monitoring the app</span></span>

<span data-ttu-id="56aa4-192">Server je instalační program předávat požadavky na `http://<serveraddress>:80` k aplikaci ASP.NET Core spuštěnou na Kestrel v `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="56aa4-192">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="56aa4-193">Nginx není však nastavit ke správě procesu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="56aa4-193">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="56aa4-194">*systemd* slouží k vytvoření souboru služby spuštění a sledování základní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="56aa4-194">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="56aa4-195">*systemd* je init systém, který poskytuje mnoho výkonné funkce pro spouštění, zastavování a Správa procesů.</span><span class="sxs-lookup"><span data-stu-id="56aa4-195">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="56aa4-196">Vytvoření souboru služby</span><span class="sxs-lookup"><span data-stu-id="56aa4-196">Create the service file</span></span>

<span data-ttu-id="56aa4-197">Vytvořte soubor definice služby:</span><span class="sxs-lookup"><span data-stu-id="56aa4-197">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="56aa4-198">Následující příklad je služba soubor pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="56aa4-198">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="56aa4-199">Pokud uživatel *www-data* nepoužívá konfigurace, uživatelsky definované v tomto poli musí být nejprve a pro soubory zadané správné vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="56aa4-199">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="56aa4-200">Linux má systém souborů malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="56aa4-200">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="56aa4-201">Nastavení ASPNETCORE_ENVIRONMENT "Výroba" hledání konfiguračního souboru *appsettings. Production.JSON*, nikoli *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="56aa4-201">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="56aa4-202">Některé hodnoty (například připojovací řetězce SQL), je nutné uvést pro zprostředkovatele konfigurace pro čtení proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="56aa4-202">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="56aa4-203">Použijte následující příkaz k vygenerování správně uvozený hodnoty pro použití v konfiguračním souboru:</span><span class="sxs-lookup"><span data-stu-id="56aa4-203">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="56aa4-204">Uložte tento soubor a povolení služby.</span><span class="sxs-lookup"><span data-stu-id="56aa4-204">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="56aa4-205">Spusťte službu a ověřte, zda je spuštěna.</span><span class="sxs-lookup"><span data-stu-id="56aa4-205">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="56aa4-206">Reverzní proxy server nakonfigurovat a spravovat prostřednictvím systemd Kestrel, webové aplikace je plně nakonfigurovaná a je přístupný z prohlížeče v místním počítači v `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="56aa4-206">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="56aa4-207">Je také přístupné ze vzdáleného počítače, blokování žádné brány firewall, která může být blokování.</span><span class="sxs-lookup"><span data-stu-id="56aa4-207">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="56aa4-208">Probíhá kontrola hlavičky odpovědi `Server` záhlaví zobrazuje aplikace ASP.NET Core obsluhovány pomocí Kestrel.</span><span class="sxs-lookup"><span data-stu-id="56aa4-208">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="56aa4-209">Prohlížení protokolů</span><span class="sxs-lookup"><span data-stu-id="56aa4-209">Viewing logs</span></span>

<span data-ttu-id="56aa4-210">Od webové aplikace pomocí Kestrel je spravovat pomocí `systemd`, všechny události a procesy, které jsou zaznamenány do centralizované deníku.</span><span class="sxs-lookup"><span data-stu-id="56aa4-210">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="56aa4-211">Ale tento deník obsahuje všechny položky pro všechny služby a spravuje procesy `systemd`.</span><span class="sxs-lookup"><span data-stu-id="56aa4-211">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="56aa4-212">Chcete-li zobrazit `kestrel-hellomvc.service`-konkrétní položky, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="56aa4-212">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="56aa4-213">Pro další, čas možnosti filtrování, jako `--since today`, `--until 1 hour ago` nebo kombinaci těchto může snížit množství vrácena žádná položka.</span><span class="sxs-lookup"><span data-stu-id="56aa4-213">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="56aa4-214">Zabezpečení aplikací</span><span class="sxs-lookup"><span data-stu-id="56aa4-214">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="56aa4-215">Povolit AppArmor</span><span class="sxs-lookup"><span data-stu-id="56aa4-215">Enable AppArmor</span></span>

<span data-ttu-id="56aa4-216">Linux zabezpečení moduly (LSM) je rozhraní, které je součástí jádra Linux od Linux 2.6.</span><span class="sxs-lookup"><span data-stu-id="56aa4-216">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="56aa4-217">LSM podporuje různé implementace modulů zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="56aa4-217">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="56aa4-218">[AppArmor](https://wiki.ubuntu.com/AppArmor) je LSM, který implementuje systém povinné řízení přístupu, který umožní uzavírání program, který má omezenou sadu prostředků.</span><span class="sxs-lookup"><span data-stu-id="56aa4-218">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="56aa4-219">Zkontrolujte AppArmor je povolená a správně nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="56aa4-219">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="56aa4-220">Konfigurace brány firewall</span><span class="sxs-lookup"><span data-stu-id="56aa4-220">Configuring the firewall</span></span>

<span data-ttu-id="56aa4-221">Zavřete vypnout všechny externí porty, které nejsou používána.</span><span class="sxs-lookup"><span data-stu-id="56aa4-221">Close off all external ports that are not in use.</span></span> <span data-ttu-id="56aa4-222">Nekomplikované brány firewall (ufw) poskytuje front-end pro `iptables` tím, že poskytuje rozhraní příkazového řádku pro konfiguraci brány firewall.</span><span class="sxs-lookup"><span data-stu-id="56aa4-222">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="56aa4-223">Ověřte, že `ufw` nastaven tak, aby povolovala přenosy na všechny porty potřebné.</span><span class="sxs-lookup"><span data-stu-id="56aa4-223">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="56aa4-224">Zabezpečení Nginx</span><span class="sxs-lookup"><span data-stu-id="56aa4-224">Securing Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="56aa4-225">Změňte název Nginx odpovědi</span><span class="sxs-lookup"><span data-stu-id="56aa4-225">Change the Nginx response name</span></span>

<span data-ttu-id="56aa4-226">Edit *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="56aa4-226">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="56aa4-227">Konfigurace možností</span><span class="sxs-lookup"><span data-stu-id="56aa4-227">Configure options</span></span>

<span data-ttu-id="56aa4-228">Konfigurace serveru s další požadované moduly.</span><span class="sxs-lookup"><span data-stu-id="56aa4-228">Configure the server with additional required modules.</span></span> <span data-ttu-id="56aa4-229">Zvažte použití brány firewall webových aplikací, jako například [ModSecurity](https://www.modsecurity.org/), k posílení zabezpečení aplikace.</span><span class="sxs-lookup"><span data-stu-id="56aa4-229">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="configure-ssl"></a><span data-ttu-id="56aa4-230">Konfigurace protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="56aa4-230">Configure SSL</span></span>

* <span data-ttu-id="56aa4-231">Konfigurace serveru pro naslouchání na přenosy HTTPS na portu `443` zadáním platný certifikát vydaný důvěryhodné certifikační autority (CA).</span><span class="sxs-lookup"><span data-stu-id="56aa4-231">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="56aa4-232">Posílení zabezpečení díky využití architektury některé postupy znázorněn v následujícím */etc/nginx/nginx.conf* souboru.</span><span class="sxs-lookup"><span data-stu-id="56aa4-232">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="56aa4-233">Mezi příklady patří výběr silnější šifrování a přesměrování veškerý provoz prostřednictvím protokolu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56aa4-233">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="56aa4-234">Přidání `HTTP Strict-Transport-Security` hlavičky (HSTS) zajišťuje, všechny následné žádosti od klienta jsou jenom přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56aa4-234">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="56aa4-235">Nepřidáte záhlaví zabezpečení přenosu Strict nebo zvolit odpovídající `max-age` Pokud bude v budoucnu zakázán protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="56aa4-235">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="56aa4-236">Přidat */etc/nginx/proxy.conf* konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="56aa4-236">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="56aa4-237">Upravit */etc/nginx/nginx.conf* konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="56aa4-237">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="56aa4-238">V příkladu obsahuje oba `http` a `server` oddílů v souboru jednu konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="56aa4-238">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="56aa4-239">Zabezpečený Nginx z útoků typu clickjacking</span><span class="sxs-lookup"><span data-stu-id="56aa4-239">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="56aa4-240">Útoků typu Clickjacking je škodlivý technika ke shromažďování nakažené uživatel klikne na.</span><span class="sxs-lookup"><span data-stu-id="56aa4-240">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="56aa4-241">Útoků typu Clickjacking triky postižené (návštěvníka) do kliknutím na nakažené lokality.</span><span class="sxs-lookup"><span data-stu-id="56aa4-241">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="56aa4-242">Použití X-FRAME-OPTIONS k zabezpečení webu.</span><span class="sxs-lookup"><span data-stu-id="56aa4-242">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="56aa4-243">Upravit *nginx.conf* souboru:</span><span class="sxs-lookup"><span data-stu-id="56aa4-243">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="56aa4-244">Přidejte řádek `add_header X-Frame-Options "SAMEORIGIN";` a uložte soubor a pak znovu spusťte Nginx.</span><span class="sxs-lookup"><span data-stu-id="56aa4-244">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="56aa4-245">Sledování toku dat typ MIME</span><span class="sxs-lookup"><span data-stu-id="56aa4-245">MIME-type sniffing</span></span>

<span data-ttu-id="56aa4-246">Tuto hlavičku zabraňuje většina prohlížečů z sledování toku dat MIME odpověď od deklarovaný typ obsahu, jako hlavičku dostane pokyn, nechcete přepsat typ obsahu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="56aa4-246">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="56aa4-247">Pomocí `nosniff` možnost, pokud server uvádí obsah je "text/html", v prohlížeči se vykreslí jako "text/html".</span><span class="sxs-lookup"><span data-stu-id="56aa4-247">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="56aa4-248">Upravit *nginx.conf* souboru:</span><span class="sxs-lookup"><span data-stu-id="56aa4-248">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="56aa4-249">Přidejte řádek `add_header X-Content-Type-Options "nosniff";` a uložte soubor a pak znovu spusťte Nginx.</span><span class="sxs-lookup"><span data-stu-id="56aa4-249">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56aa4-250">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="56aa4-250">Additional resources</span></span>

* [<span data-ttu-id="56aa4-251">Nginx: Binární verze: oficiální Debian/Ubuntu balíčky</span><span class="sxs-lookup"><span data-stu-id="56aa4-251">Nginx: Binary Releases: Official Debian/Ubuntu packages</span></span>](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* [<span data-ttu-id="56aa4-252">Konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="56aa4-252">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="56aa4-253">NGINX: Hlavička předané pomocí</span><span class="sxs-lookup"><span data-stu-id="56aa4-253">NGINX: Using the Forwarded header</span></span>](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
