---
title: "Hostování v systému Linux s Nginx ASP.NET Core"
author: rick-anderson
description: "Popisuje, jak nastavit jako reverzní proxy server na Ubuntu 16.04 pro přenos dat protokolu HTTP do webové aplikace ASP.NET Core systémem Kestrel Nginx."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 5e85cf909c1a360f245bcc83233ccc1347735b26
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="00136-103">Hostování v systému Linux s Nginx ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="00136-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="00136-104">Podle [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="00136-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="00136-105">Tato příručka vysvětluje na serveru 16.04 Ubuntu nastavení prostředí ASP.NET Core produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="00136-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="00136-106">**Poznámka:** pro Ubuntu 14.04 *supervisord* se doporučuje jako řešení pro sledování, zpracovávají Kestrel.</span><span class="sxs-lookup"><span data-stu-id="00136-106">**Note:** For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="00136-107">*systemd* na Ubuntu 14.04 není dostupný.</span><span class="sxs-lookup"><span data-stu-id="00136-107">*systemd* isn't available on Ubuntu 14.04.</span></span> [<span data-ttu-id="00136-108">Viz předchozí verze tohoto dokumentu</span><span class="sxs-lookup"><span data-stu-id="00136-108">See previous version of this document</span></span>](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

<span data-ttu-id="00136-109">Tato příručka:</span><span class="sxs-lookup"><span data-stu-id="00136-109">This guide:</span></span>

* <span data-ttu-id="00136-110">Umístí stávající aplikace ASP.NET Core za reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="00136-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="00136-111">Nastaví reverzní proxy server pro předávání požadavků na Kestrel webový server.</span><span class="sxs-lookup"><span data-stu-id="00136-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="00136-112">Zajišťuje, že webová aplikace spuštěna při spuštění jako démon.</span><span class="sxs-lookup"><span data-stu-id="00136-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="00136-113">Konfiguruje nástroj pro správu procesu pomohou restartování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="00136-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00136-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="00136-114">Prerequisites</span></span>

1. <span data-ttu-id="00136-115">Přístup k serveru 16.04 Ubuntu s standardní uživatelský účet s oprávněním sudo</span><span class="sxs-lookup"><span data-stu-id="00136-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="00136-116">Stávající aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="00136-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="00136-117">Kopírování prostřednictvím aplikace</span><span class="sxs-lookup"><span data-stu-id="00136-117">Copy over the app</span></span>

<span data-ttu-id="00136-118">Spustit [dotnet publikování](/dotnet/core/tools/dotnet-publish) z prostředí vývojářů pro zabalení aplikace do samostatný adresář, který můžete spustit na serveru.</span><span class="sxs-lookup"><span data-stu-id="00136-118">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="00136-119">Kopie aplikace ASP.NET Core k serveru pomocí ať nástroj integruje do pracovního postupu organizace (například spojovací bod služby, FTP).</span><span class="sxs-lookup"><span data-stu-id="00136-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="00136-120">Testování aplikace, například:</span><span class="sxs-lookup"><span data-stu-id="00136-120">Test the app, for example:</span></span>

* <span data-ttu-id="00136-121">Z příkazového řádku, spusťte `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="00136-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="00136-122">V prohlížeči přejděte na `http://<serveraddress>:<port>` k ověření, že aplikace funguje v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="00136-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="00136-123">Konfigurace serveru reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="00136-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="00136-124">Reverzní proxy server je běžné instalační program pro obsluhující dynamické webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="00136-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="00136-125">Reverzní proxy server ukončí požadavek HTTP a předává do aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="00136-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="00136-126">Proč používat reverzní proxy server?</span><span class="sxs-lookup"><span data-stu-id="00136-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="00136-127">Kestrel je skvělá pro obsluhující dynamický obsah z ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="00136-127">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="00136-128">Však nejsou webové funkce slouží jako bohaté funkce jako servery, například služby IIS, Apache nebo Nginx.</span><span class="sxs-lookup"><span data-stu-id="00136-128">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="00136-129">Reverzní proxy server můžete přesměrovat pracovní například statický obsah obsluhuje, ukládání do mezipaměti požadavky, komprese požadavků a ukončení protokolu SSL ze serveru HTTP.</span><span class="sxs-lookup"><span data-stu-id="00136-129">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="00136-130">Reverzní proxy server může být na vyhrazeném počítači nebo může být nasazeny společně se HTTP server.</span><span class="sxs-lookup"><span data-stu-id="00136-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="00136-131">Pro účely tohoto průvodce se používá jednu instanci Nginx.</span><span class="sxs-lookup"><span data-stu-id="00136-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="00136-132">Běží na stejném serveru, spolu s HTTP server.</span><span class="sxs-lookup"><span data-stu-id="00136-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="00136-133">Na základě požadavků, může být zvolen jiné instalace.</span><span class="sxs-lookup"><span data-stu-id="00136-133">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="00136-134">Protože požadavky jsou předávány podle reverzní proxy server, použijte hlavičky Middleware předávány z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="00136-134">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="00136-135">Middleware aktualizace `Request.Scheme`pomocí `X-Forwarded-Proto` záhlaví, tak, že přesměrování identifikátory URI a jiné zásady zabezpečení pracovat správně.</span><span class="sxs-lookup"><span data-stu-id="00136-135">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="00136-136">Při použití jakéhokoli typu middleware ověřování, musíte spustit první Middleware předávaných hlavičky.</span><span class="sxs-lookup"><span data-stu-id="00136-136">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="00136-137">Toto uspořádání zajistí, že ověřovací middleware může spotřebovávat hodnoty hlavičky a generovat správné přesměrování identifikátory URI.</span><span class="sxs-lookup"><span data-stu-id="00136-137">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="00136-138">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="00136-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="00136-139">Vyvolání [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metoda v `Startup.Configure` před voláním [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) nebo podobné middleware ověřování schématu:</span><span class="sxs-lookup"><span data-stu-id="00136-139">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="00136-140">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="00136-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="00136-141">Vyvolání [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metoda v `Startup.Configure` před voláním [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) a [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) nebo podobné schéma ověřování Middleware:</span><span class="sxs-lookup"><span data-stu-id="00136-141">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware:</span></span>

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

<span data-ttu-id="00136-142">Pokud žádné [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) jsou určené pro middleware, jsou výchozí hlavičky předávat `None`.</span><span class="sxs-lookup"><span data-stu-id="00136-142">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

### <a name="install-nginx"></a><span data-ttu-id="00136-143">Nainstalujte Nginx</span><span class="sxs-lookup"><span data-stu-id="00136-143">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="00136-144">Pokud se nainstalují volitelné moduly Nginx, může být potřeba vytváření Nginx ze zdroje.</span><span class="sxs-lookup"><span data-stu-id="00136-144">If optional Nginx modules will be installed, building Nginx from source might be required.</span></span>

<span data-ttu-id="00136-145">Použití `apt-get` k instalaci Nginx.</span><span class="sxs-lookup"><span data-stu-id="00136-145">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="00136-146">Instalační program vytvoří skript init V systému, který spouští Nginx jako démon na spuštění systému.</span><span class="sxs-lookup"><span data-stu-id="00136-146">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="00136-147">Vzhledem k tomu, že Nginx byla nainstalována poprvé, explicitně spusťte ji spuštěním:</span><span class="sxs-lookup"><span data-stu-id="00136-147">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="00136-148">Ověřte, zda že prohlížeč zobrazí výchozí úvodní stránka pro Nginx.</span><span class="sxs-lookup"><span data-stu-id="00136-148">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="00136-149">Konfigurace Nginx</span><span class="sxs-lookup"><span data-stu-id="00136-149">Configure Nginx</span></span>

<span data-ttu-id="00136-150">Pokud chcete konfigurovat Nginx jako reverzní proxy server pro směrování požadavků do vaší aplikace ASP.NET Core, upravte `/etc/nginx/sites-available/default`.</span><span class="sxs-lookup"><span data-stu-id="00136-150">To configure Nginx as a reverse proxy to forward requests to our ASP.NET Core app, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="00136-151">Otevřete v textovém editoru a nahraďte jeho obsah následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="00136-151">Open it in a text editor, and replace the contents with the following:</span></span>

```
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="00136-152">Tento konfigurační soubor Nginx předává veřejné příchozí provoz z portu `80` na port `5000`.</span><span class="sxs-lookup"><span data-stu-id="00136-152">This Nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="00136-153">Po vytvoření konfigurace Nginx spustit `sudo nginx -t` syntaxi konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="00136-153">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="00136-154">Pokud se test souboru konfigurace je úspěšné, vynutit Nginx mohla vybrat změny spuštěním `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="00136-154">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="00136-155">Monitorování aplikace</span><span class="sxs-lookup"><span data-stu-id="00136-155">Monitoring the app</span></span>

<span data-ttu-id="00136-156">Server je instalační program předávat požadavky na `http://<serveraddress>:80` k aplikaci ASP.NET Core spuštěnou na Kestrel v `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="00136-156">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="00136-157">Nginx není však nastavit ke správě procesu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="00136-157">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="00136-158">*systemd* slouží k vytvoření souboru služby spuštění a sledování základní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="00136-158">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="00136-159">*systemd* je init systém, který poskytuje mnoho výkonné funkce pro spouštění, zastavování a Správa procesů.</span><span class="sxs-lookup"><span data-stu-id="00136-159">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="00136-160">Vytvoření souboru služby</span><span class="sxs-lookup"><span data-stu-id="00136-160">Create the service file</span></span>

<span data-ttu-id="00136-161">Vytvořte soubor definice služby:</span><span class="sxs-lookup"><span data-stu-id="00136-161">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="00136-162">Následující příklad je služba soubor pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="00136-162">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="00136-163">**Poznámka:** Pokud uživatel *www-data* nepoužívá konfigurace, uživatelsky definované v tomto poli musí být nejprve a pro soubory zadané správné vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="00136-163">**Note:** If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>
<span data-ttu-id="00136-164">**Poznámka:** Linux má systém souborů malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="00136-164">**Note:** Linux has a case-sensitive file system.</span></span> <span data-ttu-id="00136-165">Nastavení ASPNETCORE_ENVIRONMENT "Výroba" hledání konfiguračního souboru *appsettings. Production.JSON*, nikoli *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="00136-165">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="00136-166">Uložte tento soubor a povolení služby.</span><span class="sxs-lookup"><span data-stu-id="00136-166">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="00136-167">Spusťte službu a ověřte, zda je spuštěna.</span><span class="sxs-lookup"><span data-stu-id="00136-167">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="00136-168">Reverzní proxy server nakonfigurovat a spravovat prostřednictvím systemd Kestrel, webové aplikace je plně nakonfigurovaná a je přístupný z prohlížeče v místním počítači v `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="00136-168">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="00136-169">Je také přístupné ze vzdáleného počítače, blokování žádné brány firewall, která může být blokování.</span><span class="sxs-lookup"><span data-stu-id="00136-169">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="00136-170">Probíhá kontrola hlavičky odpovědi `Server` záhlaví zobrazuje aplikace ASP.NET Core obsluhovány pomocí Kestrel.</span><span class="sxs-lookup"><span data-stu-id="00136-170">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="00136-171">Prohlížení protokolů</span><span class="sxs-lookup"><span data-stu-id="00136-171">Viewing logs</span></span>

<span data-ttu-id="00136-172">Od webové aplikace pomocí Kestrel je spravovat pomocí `systemd`, všechny události a procesy, které jsou zaznamenány do centralizované deníku.</span><span class="sxs-lookup"><span data-stu-id="00136-172">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="00136-173">Ale tento deník obsahuje všechny položky pro všechny služby a spravuje procesy `systemd`.</span><span class="sxs-lookup"><span data-stu-id="00136-173">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="00136-174">Chcete-li zobrazit `kestrel-hellomvc.service`-konkrétní položky, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="00136-174">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="00136-175">Pro další, čas možnosti filtrování, jako `--since today`, `--until 1 hour ago` nebo kombinaci těchto může snížit množství vrácena žádná položka.</span><span class="sxs-lookup"><span data-stu-id="00136-175">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="00136-176">Zabezpečení aplikací</span><span class="sxs-lookup"><span data-stu-id="00136-176">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="00136-177">Povolit AppArmor</span><span class="sxs-lookup"><span data-stu-id="00136-177">Enable AppArmor</span></span>

<span data-ttu-id="00136-178">Linux zabezpečení moduly (LSM) je rozhraní, které je součástí jádra Linux od Linux 2.6.</span><span class="sxs-lookup"><span data-stu-id="00136-178">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="00136-179">LSM podporuje různé implementace modulů zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="00136-179">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="00136-180">[AppArmor](https://wiki.ubuntu.com/AppArmor) je LSM, který implementuje systém povinné řízení přístupu, který umožní uzavírání program, který má omezenou sadu prostředků.</span><span class="sxs-lookup"><span data-stu-id="00136-180">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="00136-181">Zkontrolujte AppArmor je povolená a správně nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="00136-181">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="00136-182">Konfigurace brány firewall</span><span class="sxs-lookup"><span data-stu-id="00136-182">Configuring the firewall</span></span>

<span data-ttu-id="00136-183">Zavřete vypnout všechny externí porty, které nejsou používána.</span><span class="sxs-lookup"><span data-stu-id="00136-183">Close off all external ports that are not in use.</span></span> <span data-ttu-id="00136-184">Nekomplikované brány firewall (ufw) poskytuje front-end pro `iptables` tím, že poskytuje rozhraní příkazového řádku pro konfiguraci brány firewall.</span><span class="sxs-lookup"><span data-stu-id="00136-184">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="00136-185">Ověřte, že `ufw` nastaven tak, aby povolovala přenosy na všechny porty potřebné.</span><span class="sxs-lookup"><span data-stu-id="00136-185">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="00136-186">Zabezpečení Nginx</span><span class="sxs-lookup"><span data-stu-id="00136-186">Securing Nginx</span></span>

<span data-ttu-id="00136-187">Výchozí distribuci Nginx není povolit protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="00136-187">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="00136-188">Pokud chcete povolit další funkce zabezpečení, sestavení ze zdroje.</span><span class="sxs-lookup"><span data-stu-id="00136-188">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="00136-189">Stáhnout zdrojovou verzi a instalaci závislostí sestavení</span><span class="sxs-lookup"><span data-stu-id="00136-189">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="00136-190">Změňte název Nginx odpovědi</span><span class="sxs-lookup"><span data-stu-id="00136-190">Change the Nginx response name</span></span>

<span data-ttu-id="00136-191">Edit *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="00136-191">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="00136-192">Nakonfigurujte možnosti a sestavení</span><span class="sxs-lookup"><span data-stu-id="00136-192">Configure the options and build</span></span>

<span data-ttu-id="00136-193">Knihovna PCRE je vyžadována pro regulární výrazy.</span><span class="sxs-lookup"><span data-stu-id="00136-193">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="00136-194">Regulární výrazy se používají v direktivě umístění pro ngx_http_rewrite_module.</span><span class="sxs-lookup"><span data-stu-id="00136-194">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="00136-195">Http_ssl_module přidává podporu protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="00136-195">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="00136-196">Zvažte použití brány firewall webových aplikací jako *ModSecurity* k posílení zabezpečení aplikace.</span><span class="sxs-lookup"><span data-stu-id="00136-196">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="00136-197">Konfigurace protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="00136-197">Configure SSL</span></span>

* <span data-ttu-id="00136-198">Konfigurace serveru pro naslouchání na přenosy HTTPS na portu `443` zadáním platný certifikát vydaný důvěryhodné certifikační autority (CA).</span><span class="sxs-lookup"><span data-stu-id="00136-198">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="00136-199">Posílení zabezpečení díky využití architektury některé postupy znázorněn v následujícím */etc/nginx/nginx.conf* souboru.</span><span class="sxs-lookup"><span data-stu-id="00136-199">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="00136-200">Mezi příklady patří výběr silnější šifrování a přesměrování veškerý provoz prostřednictvím protokolu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="00136-200">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="00136-201">Přidání `HTTP Strict-Transport-Security` hlavičky (HSTS) zajišťuje, všechny následné žádosti od klienta jsou jenom přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="00136-201">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="00136-202">Nepřidáte záhlaví zabezpečení přenosu Strict nebo zvolit odpovídající `max-age` Pokud bude v budoucnu zakázán protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="00136-202">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="00136-203">Přidat */etc/nginx/proxy.conf* konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="00136-203">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="00136-204">Upravit */etc/nginx/nginx.conf* konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="00136-204">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="00136-205">V příkladu obsahuje oba `http` a `server` oddílů v souboru jednu konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="00136-205">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="00136-206">Zabezpečený Nginx z útoků typu clickjacking</span><span class="sxs-lookup"><span data-stu-id="00136-206">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="00136-207">Útoků typu Clickjacking je škodlivý technika ke shromažďování nakažené uživatel klikne na.</span><span class="sxs-lookup"><span data-stu-id="00136-207">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="00136-208">Útoků typu Clickjacking triky postižené (návštěvníka) do kliknutím na nakažené lokality.</span><span class="sxs-lookup"><span data-stu-id="00136-208">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="00136-209">Použití X-FRAME-OPTIONS k zabezpečení webu.</span><span class="sxs-lookup"><span data-stu-id="00136-209">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="00136-210">Upravit *nginx.conf* souboru:</span><span class="sxs-lookup"><span data-stu-id="00136-210">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="00136-211">Přidejte řádek `add_header X-Frame-Options "SAMEORIGIN";` a uložte soubor a pak znovu spusťte Nginx.</span><span class="sxs-lookup"><span data-stu-id="00136-211">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="00136-212">Sledování toku dat typ MIME</span><span class="sxs-lookup"><span data-stu-id="00136-212">MIME-type sniffing</span></span>

<span data-ttu-id="00136-213">Tuto hlavičku zabraňuje většina prohlížečů z sledování toku dat MIME odpověď od deklarovaný typ obsahu, jako hlavičku dostane pokyn, nechcete přepsat typ obsahu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="00136-213">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="00136-214">Pomocí `nosniff` možnost, pokud server uvádí obsah je "text/html", v prohlížeči se vykreslí jako "text/html".</span><span class="sxs-lookup"><span data-stu-id="00136-214">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="00136-215">Upravit *nginx.conf* souboru:</span><span class="sxs-lookup"><span data-stu-id="00136-215">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="00136-216">Přidejte řádek `add_header X-Content-Type-Options "nosniff";` a uložte soubor a pak znovu spusťte Nginx.</span><span class="sxs-lookup"><span data-stu-id="00136-216">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
