---
title: "Hostování v systému Linux s Nginx ASP.NET Core"
description: "Popisuje, jak nastavit jako reverzní proxy server na Ubuntu 16.04 pro přenos dat protokolu HTTP k webové aplikaci ASP.NET Core systémem Kestrel Nginx."
keywords: ASP.NET Core, Linux, nginx, Ubuntu, Reverse Proxy
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 08/21/2017
ms.topic: article
ms.assetid: 1c33e576-33de-481a-8ad3-896b94fde0e3
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/linuxproduction
ms.openlocfilehash: 7c7b949fc922c605aa4554c158200a4123c4eb1c
ms.sourcegitcommit: fc98e93464ccf37d9904e89a71cdddbd4bbdb86a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a><span data-ttu-id="58490-104">Nastavení hostitelského prostředí pro ASP.NET Core v systému Linux s Nginx a nasazení do ní</span><span class="sxs-lookup"><span data-stu-id="58490-104">Set up a hosting environment for ASP.NET Core on Linux with Nginx, and deploy to it</span></span>

<span data-ttu-id="58490-105">Podle [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="58490-105">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="58490-106">Tato příručka vysvětluje na serveru 16.04 Ubuntu nastavení prostředí ASP.NET Core produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="58490-106">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="58490-107">**Poznámka:** pro Ubuntu 14.04, se doporučuje supervisord jako řešení pro sledování, zpracovávají Kestrel.</span><span class="sxs-lookup"><span data-stu-id="58490-107">**Note:** For Ubuntu 14.04, supervisord is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="58490-108">systemd není k dispozici na Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="58490-108">systemd is not available on Ubuntu 14.04.</span></span> [<span data-ttu-id="58490-109">Viz předchozí verze tohoto dokumentu</span><span class="sxs-lookup"><span data-stu-id="58490-109">See previous version of this document</span></span>](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

<span data-ttu-id="58490-110">Tato příručka:</span><span class="sxs-lookup"><span data-stu-id="58490-110">This guide:</span></span>

* <span data-ttu-id="58490-111">Umístí existující aplikaci ASP.NET Core za reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="58490-111">Places an existing ASP.NET Core application behind a reverse proxy server</span></span>
* <span data-ttu-id="58490-112">Nastaví reverzního proxy serveru tak, aby předávat požadavky na webový server Kestrel</span><span class="sxs-lookup"><span data-stu-id="58490-112">Sets up the reverse proxy server to forward requests to the Kestrel web server</span></span>
* <span data-ttu-id="58490-113">Zajišťuje, že je webová aplikace spuštěna při spuštění jako démon</span><span class="sxs-lookup"><span data-stu-id="58490-113">Ensures the web application runs on startup as a daemon</span></span>
* <span data-ttu-id="58490-114">Konfiguruje nástroj pro správu procesu pomohou restartování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="58490-114">Configures a process management tool to help restart the web application</span></span>

## <a name="prerequisites"></a><span data-ttu-id="58490-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="58490-115">Prerequisites</span></span>

1. <span data-ttu-id="58490-116">Přístup k serveru 16.04 Ubuntu s standardní uživatelský účet s oprávněním sudo</span><span class="sxs-lookup"><span data-stu-id="58490-116">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="58490-117">Existující aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="58490-117">An existing ASP.NET Core application</span></span>

## <a name="copy-over-your-app"></a><span data-ttu-id="58490-118">Zkopírujte nad vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="58490-118">Copy over your app</span></span>

<span data-ttu-id="58490-119">Spustit `dotnet publish` z prostředí vývojářů pro zabalení aplikace do samostatný adresář, který můžete spustit na serveru.</span><span class="sxs-lookup"><span data-stu-id="58490-119">Run `dotnet publish` from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="58490-120">Zkopírujte aplikace ASP.NET Core k serveru pomocí ať nástroj (spojovací bod služby, FTP, atd.) se integruje do vašeho pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="58490-120">Copy the ASP.NET Core app to the server using whatever tool (SCP, FTP, etc.) integrates into your workflow.</span></span> <span data-ttu-id="58490-121">Testování aplikace, například:</span><span class="sxs-lookup"><span data-stu-id="58490-121">Test the app, for example:</span></span>
 - <span data-ttu-id="58490-122">Z příkazového řádku, spusťte`dotnet yourapp.dll`</span><span class="sxs-lookup"><span data-stu-id="58490-122">From the command line, run `dotnet yourapp.dll`</span></span>
 - <span data-ttu-id="58490-123">V prohlížeči přejděte na `http://<serveraddress>:<port>` k ověření, že aplikace funguje v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="58490-123">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="58490-124">Konfigurace serveru reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="58490-124">Configure a reverse proxy server</span></span>

<span data-ttu-id="58490-125">Reverzní proxy server je běžné instalační program pro obsluhující dynamické webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="58490-125">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="58490-126">Reverzní proxy server ukončí požadavek HTTP a předává do aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="58490-126">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core application.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="58490-127">Proč používat reverzní proxy server?</span><span class="sxs-lookup"><span data-stu-id="58490-127">Why use a reverse proxy server?</span></span>

<span data-ttu-id="58490-128">Kestrel je skvělá pro obsluhující dynamický obsah z ASP.NET Core; však nejsou webové části slouží jako bohaté funkce jako servery jako služby IIS, Apache nebo Nginx.</span><span class="sxs-lookup"><span data-stu-id="58490-128">Kestrel is great for serving dynamic content from ASP.NET Core; however, the web serving parts aren’t as feature rich as servers like IIS, Apache, or Nginx.</span></span> <span data-ttu-id="58490-129">Reverzní proxy server můžete přesměrovat pracovní obsluhující statický obsah, ukládání do mezipaměti požadavky, komprese požadavků a ukončení protokolu SSL ze serveru HTTP.</span><span class="sxs-lookup"><span data-stu-id="58490-129">A reverse proxy server can offload work like serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="58490-130">Reverzní proxy server může být na vyhrazeném počítači nebo může být nasazeny společně se HTTP server.</span><span class="sxs-lookup"><span data-stu-id="58490-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="58490-131">Pro účely tohoto průvodce se používá jednu instanci Nginx.</span><span class="sxs-lookup"><span data-stu-id="58490-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="58490-132">Běží na stejném serveru, spolu s HTTP server.</span><span class="sxs-lookup"><span data-stu-id="58490-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="58490-133">Podle potřeb, můžete zvolit jiné instalace.</span><span class="sxs-lookup"><span data-stu-id="58490-133">Based on your requirements, you may choose a different setup.</span></span>

<span data-ttu-id="58490-134">Protože požadavky jsou předávány podle reverzní proxy server, použijte `ForwardedHeaders` middleware z `Microsoft.AspNetCore.HttpOverrides` balíčku.</span><span class="sxs-lookup"><span data-stu-id="58490-134">Because requests are forwarded by reverse proxy, use the `ForwardedHeaders` middleware from the `Microsoft.AspNetCore.HttpOverrides` package.</span></span> <span data-ttu-id="58490-135">Tento middleware aktualizace `Request.Scheme`pomocí `X-Forwarded-Proto` záhlaví, tak, že přesměrování identifikátory URI a jiné zásady zabezpečení pracovat správně.</span><span class="sxs-lookup"><span data-stu-id="58490-135">This middleware updates `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="58490-136">Při nastavování reverzní proxy server, musí middleware ověřování `UseForwardedHeaders` má spustit jako první.</span><span class="sxs-lookup"><span data-stu-id="58490-136">When setting up a reverse proxy server, the authentication middleware needs `UseForwardedHeaders` to run first.</span></span> <span data-ttu-id="58490-137">Toto uspořádání zajistí, že ověřovací middleware může spotřebovávat ovlivněných hodnoty a generovat správné přesměrování identifikátory URI.</span><span class="sxs-lookup"><span data-stu-id="58490-137">This ordering ensures that the authentication middleware can consume the affected values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="58490-138">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="58490-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="58490-139">Vyvolání `UseForwardedHeaders` – metoda (v `Configure` metodu *Startup.cs*) před voláním `UseAuthentication` nebo podobné middleware ověřování schématu:</span><span class="sxs-lookup"><span data-stu-id="58490-139">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="58490-140">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="58490-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="58490-141">Vyvolání `UseForwardedHeaders` – metoda (v `Configure` metodu *Startup.cs*) před voláním `UseIdentity` a `UseFacebookAuthentication` nebo podobné middleware ověřování schématu:</span><span class="sxs-lookup"><span data-stu-id="58490-141">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseIdentity` and `UseFacebookAuthentication` or similar authentication scheme middleware:</span></span>

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

### <a name="install-nginx"></a><span data-ttu-id="58490-142">Nainstalujte Nginx</span><span class="sxs-lookup"><span data-stu-id="58490-142">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="58490-143">Pokud máte v plánu pro instalaci volitelné moduly Nginx, může být nutné vytvořit Nginx ze zdroje.</span><span class="sxs-lookup"><span data-stu-id="58490-143">If you plan to install optional Nginx modules, you may be required to build Nginx from source.</span></span>

<span data-ttu-id="58490-144">Použití `apt-get` k instalaci Nginx.</span><span class="sxs-lookup"><span data-stu-id="58490-144">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="58490-145">Instalační program vytvoří skript init V systému, který spouští Nginx jako démon na spuštění systému.</span><span class="sxs-lookup"><span data-stu-id="58490-145">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="58490-146">Vzhledem k tomu, že Nginx byla nainstalována poprvé, explicitně spusťte ji spuštěním:</span><span class="sxs-lookup"><span data-stu-id="58490-146">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="58490-147">Ověřte, zda že prohlížeč zobrazí výchozí úvodní stránka pro Nginx.</span><span class="sxs-lookup"><span data-stu-id="58490-147">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="58490-148">Konfigurace Nginx</span><span class="sxs-lookup"><span data-stu-id="58490-148">Configure Nginx</span></span>

<span data-ttu-id="58490-149">Pokud chcete konfigurovat Nginx jako reverzní proxy server pro směrování požadavků na aplikace ASP.NET Core, upravte `/etc/nginx/sites-available/default`.</span><span class="sxs-lookup"><span data-stu-id="58490-149">To configure Nginx as a reverse proxy to forward requests to our ASP.NET Core application, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="58490-150">Otevřete v textovém editoru a nahraďte jeho obsah následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="58490-150">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
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

<span data-ttu-id="58490-151">Tento konfigurační soubor Nginx předává veřejné příchozí provoz z portu `80` na port `5000`.</span><span class="sxs-lookup"><span data-stu-id="58490-151">This Nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="58490-152">Po dokončení provádění změn konfigurace Nginx, můžete spustit `sudo nginx -t` syntaxi konfiguračních souborů.</span><span class="sxs-lookup"><span data-stu-id="58490-152">Once you have completed making changes to your Nginx configuration, you can run `sudo nginx -t` to verify the syntax of your configuration files.</span></span> <span data-ttu-id="58490-153">Pokud se test souboru konfigurace je úspěšné, požádejte Nginx mohla vybrat změny spuštěním `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="58490-153">If the configuration file test is successful, you can ask Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-our-application"></a><span data-ttu-id="58490-154">Monitorování aplikace</span><span class="sxs-lookup"><span data-stu-id="58490-154">Monitoring our application</span></span>

<span data-ttu-id="58490-155">Nginx je teď instalace předávat požadavky na `http://yourhost:80` k aplikaci ASP.NET Core systémem Kestrel v `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="58490-155">Nginx is now setup to forward requests made to `http://yourhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="58490-156">Nginx není však nastavit ke správě procesu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="58490-156">However, Nginx is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="58490-157">Můžete použít *systemd* a vytvořte soubor služby spuštění a sledování základní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="58490-157">You can use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="58490-158">*systemd* je init systém, který poskytuje mnoho výkonné funkce pro spouštění, zastavování a Správa procesů.</span><span class="sxs-lookup"><span data-stu-id="58490-158">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="58490-159">Vytvoření souboru služby</span><span class="sxs-lookup"><span data-stu-id="58490-159">Create the service file</span></span>

<span data-ttu-id="58490-160">Vytvořte soubor definice služby:</span><span class="sxs-lookup"><span data-stu-id="58490-160">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="58490-161">Následující příklad je služba soubor pro naši aplikaci:</span><span class="sxs-lookup"><span data-stu-id="58490-161">The following is an example service file for our application:</span></span>

```ini
[Unit]
Description=Example .NET Web API Application running on Ubuntu

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

<span data-ttu-id="58490-162">**Poznámka:** Pokud uživatel *www-data* nepoužívá konfigurace, uživatelsky definované v tomto poli musí být nejprve vytvoření a pro soubory zadané správné vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="58490-162">**Note:** If the user *www-data* is not used by your configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="58490-163">Uložte soubor a povolení služby.</span><span class="sxs-lookup"><span data-stu-id="58490-163">Save the file, and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="58490-164">Spusťte službu a ověřte, zda je spuštěna.</span><span class="sxs-lookup"><span data-stu-id="58490-164">Start the service and verify that it is running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API Application running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="58490-165">Reverzní proxy server nakonfigurovat a spravovat prostřednictvím systemd Kestrel, webové aplikace je plně nakonfigurovaná a je přístupný z prohlížeče v místním počítači v `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="58490-165">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="58490-166">Je také přístupné ze vzdáleného počítače, blokování žádné brány firewall, která může být blokování.</span><span class="sxs-lookup"><span data-stu-id="58490-166">It is also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="58490-167">Probíhá kontrola hlavičky odpovědi `Server` záhlaví zobrazuje aplikace ASP.NET Core obsluhovány pomocí Kestrel.</span><span class="sxs-lookup"><span data-stu-id="58490-167">Inspecting the response headers, the `Server` header shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="58490-168">Prohlížení protokolů</span><span class="sxs-lookup"><span data-stu-id="58490-168">Viewing logs</span></span>

<span data-ttu-id="58490-169">Od webové aplikace pomocí Kestrel je spravovat pomocí `systemd`, všechny události a procesy, které jsou zaznamenány do centralizované deníku.</span><span class="sxs-lookup"><span data-stu-id="58490-169">Since the web application using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="58490-170">Ale tento deník obsahuje všechny položky pro všechny služby a spravuje procesy `systemd`.</span><span class="sxs-lookup"><span data-stu-id="58490-170">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="58490-171">Chcete-li zobrazit `kestrel-hellomvc.service`-konkrétní položky, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="58490-171">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="58490-172">Pro další, čas možnosti filtrování, jako `--since today`, `--until 1 hour ago` nebo kombinaci těchto může snížit množství vrácena žádná položka.</span><span class="sxs-lookup"><span data-stu-id="58490-172">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="58490-173">Zabezpečení aplikace</span><span class="sxs-lookup"><span data-stu-id="58490-173">Securing our application</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="58490-174">Povolit AppArmor</span><span class="sxs-lookup"><span data-stu-id="58490-174">Enable AppArmor</span></span>

<span data-ttu-id="58490-175">Linux zabezpečení moduly (LSM) je rozhraní, které je součástí jádra Linux od Linux 2.6.</span><span class="sxs-lookup"><span data-stu-id="58490-175">Linux Security Modules (LSM) is a framework that is part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="58490-176">LSM podporuje různé implementace modulů zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="58490-176">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="58490-177">[AppArmor](https://wiki.ubuntu.com/AppArmor) je LSM, který implementuje systém povinné řízení přístupu, který umožní uzavírání program, který má omezenou sadu prostředků.</span><span class="sxs-lookup"><span data-stu-id="58490-177">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="58490-178">Zkontrolujte AppArmor je povolená a správně nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="58490-178">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-our-firewall"></a><span data-ttu-id="58490-179">Konfigurace naše brány firewall</span><span class="sxs-lookup"><span data-stu-id="58490-179">Configuring our firewall</span></span>

<span data-ttu-id="58490-180">Zavřete vypnout všechny externí porty, které nejsou používána.</span><span class="sxs-lookup"><span data-stu-id="58490-180">Close off all external ports that are not in use.</span></span> <span data-ttu-id="58490-181">Nekomplikované brány firewall (ufw) poskytuje front-end pro `iptables` tím, že poskytuje rozhraní příkazového řádku pro konfiguraci brány firewall.</span><span class="sxs-lookup"><span data-stu-id="58490-181">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="58490-182">Ověřte, že `ufw` nastaven tak, aby povolovala přenosy na všechny porty, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="58490-182">Verify that `ufw` is configured to allow traffic on any ports you need.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="58490-183">Zabezpečení Nginx</span><span class="sxs-lookup"><span data-stu-id="58490-183">Securing Nginx</span></span>

<span data-ttu-id="58490-184">Výchozí distribuci Nginx není povolit protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="58490-184">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="58490-185">Pokud chcete povolit další funkce zabezpečení, sestavení ze zdroje.</span><span class="sxs-lookup"><span data-stu-id="58490-185">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="58490-186">Stáhnout zdrojovou verzi a instalaci závislostí sestavení</span><span class="sxs-lookup"><span data-stu-id="58490-186">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="58490-187">Změňte název Nginx odpovědi</span><span class="sxs-lookup"><span data-stu-id="58490-187">Change the Nginx response name</span></span>

<span data-ttu-id="58490-188">Upravit *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="58490-188">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="58490-189">Nakonfigurujte možnosti a sestavení</span><span class="sxs-lookup"><span data-stu-id="58490-189">Configure the options and build</span></span>

<span data-ttu-id="58490-190">Knihovna PCRE je vyžadována pro regulární výrazy.</span><span class="sxs-lookup"><span data-stu-id="58490-190">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="58490-191">Regulární výrazy se používají v direktivě umístění pro ngx_http_rewrite_module.</span><span class="sxs-lookup"><span data-stu-id="58490-191">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="58490-192">Http_ssl_module přidává podporu protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="58490-192">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="58490-193">Zvažte použití brány firewall webových aplikací jako *ModSecurity* k posílení zabezpečení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="58490-193">Consider using a web application firewall like *ModSecurity* to harden your application.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="58490-194">Konfigurace protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="58490-194">Configure SSL</span></span>

* <span data-ttu-id="58490-195">Konfigurace serveru pro naslouchání na přenosy HTTPS na portu `443` zadáním platný certifikát vydaný důvěryhodné certifikační autority (CA).</span><span class="sxs-lookup"><span data-stu-id="58490-195">Configure your server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="58490-196">Posílení zabezpečení díky využití architektury některé postupy znázorněn v následujícím */etc/nginx/nginx.conf* souboru.</span><span class="sxs-lookup"><span data-stu-id="58490-196">Harden your security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="58490-197">Mezi příklady patří výběr silnější šifrování a přesměrování veškerý provoz prostřednictvím protokolu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="58490-197">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="58490-198">Přidání `HTTP Strict-Transport-Security` hlavičky (HSTS) zajišťuje, všechny následné žádosti od klienta jsou jenom přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="58490-198">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="58490-199">Nepřidávejte záhlaví zabezpečení přenosu Strict nebo zvolit odpovídající `max-age` Pokud budete chtít v budoucnu zakázat protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="58490-199">Do not add the Strict-Transport-Security header or chose an appropriate `max-age` if you plan to disable SSL in the future.</span></span>

<span data-ttu-id="58490-200">Přidat */etc/nginx/proxy.conf* konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="58490-200">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[Main](linuxproduction/proxy.conf)]

<span data-ttu-id="58490-201">Upravit */etc/nginx/nginx.conf* konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="58490-201">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="58490-202">V příkladu obsahuje oba `http` a `server` oddílů v souboru jednu konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="58490-202">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="58490-203">Zabezpečený Nginx z útoků typu clickjacking</span><span class="sxs-lookup"><span data-stu-id="58490-203">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="58490-204">Útoků typu Clickjacking je škodlivý technika ke shromažďování nakažené uživatel klikne na.</span><span class="sxs-lookup"><span data-stu-id="58490-204">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="58490-205">Útoků typu Clickjacking triky postižené (návštěvníka) do kliknutím na nakažené lokality.</span><span class="sxs-lookup"><span data-stu-id="58490-205">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="58490-206">Použití X-FRAME-OPTIONS pro zabezpečení vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="58490-206">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="58490-207">Upravit *nginx.conf* souboru:</span><span class="sxs-lookup"><span data-stu-id="58490-207">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="58490-208">Přidejte řádek `add_header X-Frame-Options "SAMEORIGIN";` a uložte soubor a pak znovu spusťte Nginx.</span><span class="sxs-lookup"><span data-stu-id="58490-208">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="58490-209">Sledování toku dat typ MIME</span><span class="sxs-lookup"><span data-stu-id="58490-209">MIME-type sniffing</span></span>

<span data-ttu-id="58490-210">Tuto hlavičku zabraňuje většina prohlížečů z sledování toku dat MIME odpověď od deklarovaný typ obsahu, jako hlavičku dostane pokyn, nechcete přepsat typ obsahu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="58490-210">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="58490-211">Pomocí `nosniff` možnost, pokud server uvádí obsah je "text/html", v prohlížeči se vykreslí jako "text/html".</span><span class="sxs-lookup"><span data-stu-id="58490-211">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="58490-212">Upravit *nginx.conf* souboru:</span><span class="sxs-lookup"><span data-stu-id="58490-212">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="58490-213">Přidejte řádek `add_header X-Content-Type-Options "nosniff";` a uložte soubor a pak znovu spusťte Nginx.</span><span class="sxs-lookup"><span data-stu-id="58490-213">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
