---
title: "Hostitel s nginx ASP.NET Core v systému Linux"
description: "Popisuje, jak nastavit jako reverzní proxy server na Ubuntu 16.04 pro přenos dat protokolu HTTP do webové aplikace ASP.NET Core systémem Kestrel nginx."
author: rick-anderson
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 08/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: bad84b8c68bd0bc63bcd125e1873bc99a2ed2afd
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="c45a4-103">Hostitel s nginx ASP.NET Core v systému Linux</span><span class="sxs-lookup"><span data-stu-id="c45a4-103">Host ASP.NET Core on Linux with nginx</span></span>

<span data-ttu-id="c45a4-104">Podle [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="c45a4-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="c45a4-105">Tato příručka vysvětluje na serveru 16.04 Ubuntu nastavení prostředí ASP.NET Core produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="c45a4-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="c45a4-106">**Poznámka:** pro Ubuntu 14.04 *supervisord* se doporučuje jako řešení pro sledování, zpracovávají Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c45a4-106">**Note:** For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="c45a4-107">*systemd* na Ubuntu 14.04 není dostupný.</span><span class="sxs-lookup"><span data-stu-id="c45a4-107">*systemd* isn't available on Ubuntu 14.04.</span></span> [<span data-ttu-id="c45a4-108">Viz předchozí verze tohoto dokumentu</span><span class="sxs-lookup"><span data-stu-id="c45a4-108">See previous version of this document</span></span>](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

<span data-ttu-id="c45a4-109">Tato příručka:</span><span class="sxs-lookup"><span data-stu-id="c45a4-109">This guide:</span></span>

* <span data-ttu-id="c45a4-110">Umístí stávající aplikace ASP.NET Core za reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="c45a4-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="c45a4-111">Nastaví reverzní proxy server pro předávání požadavků na Kestrel webový server.</span><span class="sxs-lookup"><span data-stu-id="c45a4-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="c45a4-112">Zajišťuje, že webová aplikace spuštěna při spuštění jako démon.</span><span class="sxs-lookup"><span data-stu-id="c45a4-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="c45a4-113">Konfiguruje nástroj pro správu procesu pomohou restartování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c45a4-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c45a4-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c45a4-114">Prerequisites</span></span>

1. <span data-ttu-id="c45a4-115">Přístup k serveru 16.04 Ubuntu s standardní uživatelský účet s oprávněním sudo</span><span class="sxs-lookup"><span data-stu-id="c45a4-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="c45a4-116">Stávající aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c45a4-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="c45a4-117">Kopírování prostřednictvím aplikace</span><span class="sxs-lookup"><span data-stu-id="c45a4-117">Copy over the app</span></span>

<span data-ttu-id="c45a4-118">Spustit `dotnet publish` z prostředí vývojářů pro zabalení aplikace do samostatný adresář, který můžete spustit na serveru.</span><span class="sxs-lookup"><span data-stu-id="c45a4-118">Run `dotnet publish` from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="c45a4-119">Kopie aplikace ASP.NET Core k serveru pomocí ať nástroj integruje do pracovního postupu organizace (například spojovací bod služby, FTP).</span><span class="sxs-lookup"><span data-stu-id="c45a4-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="c45a4-120">Testování aplikace, například:</span><span class="sxs-lookup"><span data-stu-id="c45a4-120">Test the app, for example:</span></span>

* <span data-ttu-id="c45a4-121">Z příkazového řádku, spusťte `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="c45a4-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="c45a4-122">V prohlížeči přejděte na `http://<serveraddress>:<port>` k ověření, že aplikace funguje v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="c45a4-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="c45a4-123">Konfigurace serveru reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="c45a4-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="c45a4-124">Reverzní proxy server je běžné instalační program pro obsluhující dynamické webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c45a4-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="c45a4-125">Reverzní proxy server ukončí požadavek HTTP a předává do aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c45a4-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="c45a4-126">Proč používat reverzní proxy server?</span><span class="sxs-lookup"><span data-stu-id="c45a4-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="c45a4-127">Kestrel je skvělá pro obsluhující dynamický obsah z ASP.NET Core; však nejsou webové části slouží jako bohaté funkce jako servery jako služby IIS, Apache nebo nginx.</span><span class="sxs-lookup"><span data-stu-id="c45a4-127">Kestrel is great for serving dynamic content from ASP.NET Core; however, the web serving parts aren’t as feature rich as servers like IIS, Apache, or nginx.</span></span> <span data-ttu-id="c45a4-128">Reverzní proxy server můžete přesměrovat pracovní obsluhující statický obsah, ukládání do mezipaměti požadavky, komprese požadavků a ukončení protokolu SSL ze serveru HTTP.</span><span class="sxs-lookup"><span data-stu-id="c45a4-128">A reverse proxy server can offload work like serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="c45a4-129">Reverzní proxy server může být na vyhrazeném počítači nebo může být nasazeny společně se HTTP server.</span><span class="sxs-lookup"><span data-stu-id="c45a4-129">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="c45a4-130">Pro účely tohoto průvodce se používá jednu instanci nginx.</span><span class="sxs-lookup"><span data-stu-id="c45a4-130">For the purposes of this guide, a single instance of nginx is used.</span></span> <span data-ttu-id="c45a4-131">Běží na stejném serveru, spolu s HTTP server.</span><span class="sxs-lookup"><span data-stu-id="c45a4-131">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="c45a4-132">Na základě požadavků, různé instalační může být zvolené.</span><span class="sxs-lookup"><span data-stu-id="c45a4-132">Based on requirements, a different setup may be choosen.</span></span>

<span data-ttu-id="c45a4-133">Protože požadavky jsou předávány podle reverzní proxy server, použijte `ForwardedHeaders` middleware z `Microsoft.AspNetCore.HttpOverrides` balíčku.</span><span class="sxs-lookup"><span data-stu-id="c45a4-133">Because requests are forwarded by reverse proxy, use the `ForwardedHeaders` middleware from the `Microsoft.AspNetCore.HttpOverrides` package.</span></span> <span data-ttu-id="c45a4-134">Tento middleware aktualizace `Request.Scheme`pomocí `X-Forwarded-Proto` záhlaví, tak, že přesměrování identifikátory URI a jiné zásady zabezpečení pracovat správně.</span><span class="sxs-lookup"><span data-stu-id="c45a4-134">This middleware updates `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="c45a4-135">Při nastavování reverzní proxy server, musí middleware ověřování `UseForwardedHeaders` má spustit jako první.</span><span class="sxs-lookup"><span data-stu-id="c45a4-135">When setting up a reverse proxy server, the authentication middleware needs `UseForwardedHeaders` to run first.</span></span> <span data-ttu-id="c45a4-136">Toto uspořádání zajistí, že ověřovací middleware může spotřebovávat ovlivněných hodnoty a generovat správné přesměrování identifikátory URI.</span><span class="sxs-lookup"><span data-stu-id="c45a4-136">This ordering ensures that the authentication middleware can consume the affected values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c45a4-137">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="c45a4-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c45a4-138">Vyvolání `UseForwardedHeaders` – metoda (v `Configure` metodu *Startup.cs*) před voláním `UseAuthentication` nebo podobné middleware ověřování schématu:</span><span class="sxs-lookup"><span data-stu-id="c45a4-138">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c45a4-139">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="c45a4-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c45a4-140">Vyvolání `UseForwardedHeaders` – metoda (v `Configure` metodu *Startup.cs*) před voláním `UseIdentity` a `UseFacebookAuthentication` nebo podobné middleware ověřování schématu:</span><span class="sxs-lookup"><span data-stu-id="c45a4-140">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseIdentity` and `UseFacebookAuthentication` or similar authentication scheme middleware:</span></span>

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

### <a name="install-nginx"></a><span data-ttu-id="c45a4-141">Nainstalujte nginx</span><span class="sxs-lookup"><span data-stu-id="c45a4-141">Install nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="c45a4-142">Pokud se nainstalují volitelné nginx moduly, může být potřeba vytváření nginx ze zdroje.</span><span class="sxs-lookup"><span data-stu-id="c45a4-142">If optional nginx modules will be installed, building nginx from source might be required.</span></span>

<span data-ttu-id="c45a4-143">Použití `apt-get` k instalaci nginx.</span><span class="sxs-lookup"><span data-stu-id="c45a4-143">Use `apt-get` to install nginx.</span></span> <span data-ttu-id="c45a4-144">Instalační program vytvoří skript init V systému, který spouští nginx jako démon na spuštění systému.</span><span class="sxs-lookup"><span data-stu-id="c45a4-144">The installer creates a System V init script that runs nginx as daemon on system startup.</span></span> <span data-ttu-id="c45a4-145">Vzhledem k tomu, že nginx byla nainstalována poprvé, explicitně spusťte ji spuštěním:</span><span class="sxs-lookup"><span data-stu-id="c45a4-145">Since nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="c45a4-146">Ověřte, zda že prohlížeč zobrazí výchozí úvodní stránka pro nginx.</span><span class="sxs-lookup"><span data-stu-id="c45a4-146">Verify a browser displays the default landing page for nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="c45a4-147">Konfigurace nginx</span><span class="sxs-lookup"><span data-stu-id="c45a4-147">Configure nginx</span></span>

<span data-ttu-id="c45a4-148">Pokud chcete konfigurovat nginx jako reverzní proxy server pro směrování požadavků do vaší aplikace ASP.NET Core, upravte `/etc/nginx/sites-available/default`.</span><span class="sxs-lookup"><span data-stu-id="c45a4-148">To configure nginx as a reverse proxy to forward requests to our ASP.NET Core app, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="c45a4-149">Otevřete v textovém editoru a nahraďte jeho obsah následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="c45a4-149">Open it in a text editor, and replace the contents with the following:</span></span>

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

<span data-ttu-id="c45a4-150">Tento konfigurační soubor nginx předává veřejné příchozí provoz z portu `80` na port `5000`.</span><span class="sxs-lookup"><span data-stu-id="c45a4-150">This nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="c45a4-151">Po vytvoření konfigurace nginx spustit `sudo nginx -t` syntaxi konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="c45a4-151">Once the nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="c45a4-152">Pokud se test souboru konfigurace je úspěšné, vynutit nginx mohla vybrat změny spuštěním `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="c45a4-152">If the configuration file test is successful, force nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="c45a4-153">Monitorování aplikace</span><span class="sxs-lookup"><span data-stu-id="c45a4-153">Monitoring the app</span></span>

<span data-ttu-id="c45a4-154">Server je instalační program předávat požadavky na `http://<serveraddress>:80` k aplikaci ASP.NET Core spuštěnou na Kestrel v `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="c45a4-154">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="c45a4-155">Nginx není však nastavit ke správě procesu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c45a4-155">However, nginx is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="c45a4-156">*systemd* slouží k vytvoření souboru služby spuštění a sledování základní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c45a4-156">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="c45a4-157">*systemd* je init systém, který poskytuje mnoho výkonné funkce pro spouštění, zastavování a Správa procesů.</span><span class="sxs-lookup"><span data-stu-id="c45a4-157">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="c45a4-158">Vytvoření souboru služby</span><span class="sxs-lookup"><span data-stu-id="c45a4-158">Create the service file</span></span>

<span data-ttu-id="c45a4-159">Vytvořte soubor definice služby:</span><span class="sxs-lookup"><span data-stu-id="c45a4-159">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="c45a4-160">Následující příklad je služba soubor pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="c45a4-160">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="c45a4-161">**Poznámka:** Pokud uživatel *www-data* nepoužívá konfigurace, uživatelsky definované v tomto poli musí být nejprve a pro soubory zadané správné vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="c45a4-161">**Note:** If the user *www-data* is not used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="c45a4-162">Uložte tento soubor a povolení služby.</span><span class="sxs-lookup"><span data-stu-id="c45a4-162">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="c45a4-163">Spusťte službu a ověřte, zda je spuštěna.</span><span class="sxs-lookup"><span data-stu-id="c45a4-163">Start the service and verify that it is running.</span></span>

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

<span data-ttu-id="c45a4-164">Reverzní proxy server nakonfigurovat a spravovat prostřednictvím systemd Kestrel, webové aplikace je plně nakonfigurovaná a je přístupný z prohlížeče v místním počítači v `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="c45a4-164">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="c45a4-165">Je také přístupné ze vzdáleného počítače, blokování žádné brány firewall, která může být blokování.</span><span class="sxs-lookup"><span data-stu-id="c45a4-165">It is also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="c45a4-166">Probíhá kontrola hlavičky odpovědi `Server` záhlaví zobrazuje aplikace ASP.NET Core obsluhovány pomocí Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c45a4-166">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="c45a4-167">Prohlížení protokolů</span><span class="sxs-lookup"><span data-stu-id="c45a4-167">Viewing logs</span></span>

<span data-ttu-id="c45a4-168">Od webové aplikace pomocí Kestrel je spravovat pomocí `systemd`, všechny události a procesy, které jsou zaznamenány do centralizované deníku.</span><span class="sxs-lookup"><span data-stu-id="c45a4-168">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="c45a4-169">Ale tento deník obsahuje všechny položky pro všechny služby a spravuje procesy `systemd`.</span><span class="sxs-lookup"><span data-stu-id="c45a4-169">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="c45a4-170">Chcete-li zobrazit `kestrel-hellomvc.service`-konkrétní položky, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c45a4-170">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="c45a4-171">Pro další, čas možnosti filtrování, jako `--since today`, `--until 1 hour ago` nebo kombinaci těchto může snížit množství vrácena žádná položka.</span><span class="sxs-lookup"><span data-stu-id="c45a4-171">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="c45a4-172">Zabezpečení aplikací</span><span class="sxs-lookup"><span data-stu-id="c45a4-172">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="c45a4-173">Povolit AppArmor</span><span class="sxs-lookup"><span data-stu-id="c45a4-173">Enable AppArmor</span></span>

<span data-ttu-id="c45a4-174">Linux zabezpečení moduly (LSM) je rozhraní, které je součástí jádra Linux od Linux 2.6.</span><span class="sxs-lookup"><span data-stu-id="c45a4-174">Linux Security Modules (LSM) is a framework that is part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="c45a4-175">LSM podporuje různé implementace modulů zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c45a4-175">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="c45a4-176">[AppArmor](https://wiki.ubuntu.com/AppArmor) je LSM, který implementuje systém povinné řízení přístupu, který umožní uzavírání program, který má omezenou sadu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c45a4-176">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="c45a4-177">Zkontrolujte AppArmor je povolená a správně nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="c45a4-177">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="c45a4-178">Konfigurace brány firewall</span><span class="sxs-lookup"><span data-stu-id="c45a4-178">Configuring the firewall</span></span>

<span data-ttu-id="c45a4-179">Zavřete vypnout všechny externí porty, které nejsou používána.</span><span class="sxs-lookup"><span data-stu-id="c45a4-179">Close off all external ports that are not in use.</span></span> <span data-ttu-id="c45a4-180">Nekomplikované brány firewall (ufw) poskytuje front-end pro `iptables` tím, že poskytuje rozhraní příkazového řádku pro konfiguraci brány firewall.</span><span class="sxs-lookup"><span data-stu-id="c45a4-180">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="c45a4-181">Ověřte, že `ufw` nastaven tak, aby povolovala přenosy na všechny porty potřebné.</span><span class="sxs-lookup"><span data-stu-id="c45a4-181">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="c45a4-182">Zabezpečení nginx</span><span class="sxs-lookup"><span data-stu-id="c45a4-182">Securing nginx</span></span>

<span data-ttu-id="c45a4-183">Výchozí distribuci nginx není povolit protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="c45a4-183">The default distribution of nginx doesn't enable SSL.</span></span> <span data-ttu-id="c45a4-184">Pokud chcete povolit další funkce zabezpečení, sestavení ze zdroje.</span><span class="sxs-lookup"><span data-stu-id="c45a4-184">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="c45a4-185">Stáhnout zdrojovou verzi a instalaci závislostí sestavení</span><span class="sxs-lookup"><span data-stu-id="c45a4-185">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="c45a4-186">Změňte název nginx odpovědi</span><span class="sxs-lookup"><span data-stu-id="c45a4-186">Change the nginx response name</span></span>

<span data-ttu-id="c45a4-187">Upravit *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="c45a4-187">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="c45a4-188">Nakonfigurujte možnosti a sestavení</span><span class="sxs-lookup"><span data-stu-id="c45a4-188">Configure the options and build</span></span>

<span data-ttu-id="c45a4-189">Knihovna PCRE je vyžadována pro regulární výrazy.</span><span class="sxs-lookup"><span data-stu-id="c45a4-189">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="c45a4-190">Regulární výrazy se používají v direktivě umístění pro ngx_http_rewrite_module.</span><span class="sxs-lookup"><span data-stu-id="c45a4-190">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="c45a4-191">Http_ssl_module přidává podporu protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c45a4-191">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="c45a4-192">Zvažte použití brány firewall webových aplikací jako *ModSecurity* k posílení zabezpečení aplikace.</span><span class="sxs-lookup"><span data-stu-id="c45a4-192">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="c45a4-193">Konfigurace protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="c45a4-193">Configure SSL</span></span>

* <span data-ttu-id="c45a4-194">Konfigurace serveru pro naslouchání na přenosy HTTPS na portu `443` zadáním platný certifikát vydaný důvěryhodné certifikační autority (CA).</span><span class="sxs-lookup"><span data-stu-id="c45a4-194">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="c45a4-195">Posílení zabezpečení díky využití architektury některé postupy znázorněn v následujícím */etc/nginx/nginx.conf* souboru.</span><span class="sxs-lookup"><span data-stu-id="c45a4-195">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="c45a4-196">Mezi příklady patří výběr silnější šifrování a přesměrování veškerý provoz prostřednictvím protokolu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c45a4-196">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="c45a4-197">Přidání `HTTP Strict-Transport-Security` hlavičky (HSTS) zajišťuje, všechny následné žádosti od klienta jsou jenom přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c45a4-197">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="c45a4-198">Nepřidávejte záhlaví zabezpečení přenosu Strict nebo zvolit odpovídající `max-age` Pokud bude v budoucnu zakázán protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="c45a4-198">Do not add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="c45a4-199">Přidat */etc/nginx/proxy.conf* konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="c45a4-199">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[Main](linux-nginx/proxy.conf)]

<span data-ttu-id="c45a4-200">Upravit */etc/nginx/nginx.conf* konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="c45a4-200">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="c45a4-201">V příkladu obsahuje oba `http` a `server` oddílů v souboru jednu konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="c45a4-201">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[Main](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="c45a4-202">Zabezpečené nginx z útoků typu clickjacking</span><span class="sxs-lookup"><span data-stu-id="c45a4-202">Secure nginx from clickjacking</span></span>
<span data-ttu-id="c45a4-203">Útoků typu Clickjacking je škodlivý technika ke shromažďování nakažené uživatel klikne na.</span><span class="sxs-lookup"><span data-stu-id="c45a4-203">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="c45a4-204">Útoků typu Clickjacking triky postižené (návštěvníka) do kliknutím na nakažené lokality.</span><span class="sxs-lookup"><span data-stu-id="c45a4-204">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="c45a4-205">Použití X-FRAME-OPTIONS k zabezpečení webu.</span><span class="sxs-lookup"><span data-stu-id="c45a4-205">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="c45a4-206">Upravit *nginx.conf* souboru:</span><span class="sxs-lookup"><span data-stu-id="c45a4-206">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="c45a4-207">Přidejte řádek `add_header X-Frame-Options "SAMEORIGIN";` a uložte soubor a pak znovu spusťte nginx.</span><span class="sxs-lookup"><span data-stu-id="c45a4-207">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="c45a4-208">Sledování toku dat typ MIME</span><span class="sxs-lookup"><span data-stu-id="c45a4-208">MIME-type sniffing</span></span>

<span data-ttu-id="c45a4-209">Tuto hlavičku zabraňuje většina prohlížečů z sledování toku dat MIME odpověď od deklarovaný typ obsahu, jako hlavičku dostane pokyn, nechcete přepsat typ obsahu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="c45a4-209">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="c45a4-210">Pomocí `nosniff` možnost, pokud server uvádí obsah je "text/html", v prohlížeči se vykreslí jako "text/html".</span><span class="sxs-lookup"><span data-stu-id="c45a4-210">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="c45a4-211">Upravit *nginx.conf* souboru:</span><span class="sxs-lookup"><span data-stu-id="c45a4-211">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="c45a4-212">Přidejte řádek `add_header X-Content-Type-Options "nosniff";` a uložte soubor a pak znovu spusťte nginx.</span><span class="sxs-lookup"><span data-stu-id="c45a4-212">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart nginx.</span></span>
