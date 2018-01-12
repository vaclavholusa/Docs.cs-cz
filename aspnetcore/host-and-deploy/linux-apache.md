---
title: "Hostování v systému Linux s Apache ASP.NET Core"
description: "Zjistěte, jak nastavit Apache jako reverzní proxy server na CentOS a přesměrování provozu HTTP na webové aplikace ASP.NET Core systémem Kestrel."
author: spboyer
ms.author: spboyer
manager: wpickett
ms.custom: mvc
ms.date: 10/19/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/linux-apache
ms.openlocfilehash: b7cadd1b579ff99ca71e166c0e11702866d1ee24
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="00cbd-103">Hostování v systému Linux s Apache ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="00cbd-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="00cbd-104">Podle [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="00cbd-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="00cbd-105">Pomocí tohoto průvodce, zjistěte, jak nastavit [Apache](https://httpd.apache.org/) jako reverzní proxy server na [CentOS 7](https://www.centos.org/) a přesměrování provozu HTTP na webové aplikace ASP.NET Core systémem [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="00cbd-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="00cbd-106">[Mod_proxy rozšíření](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) a související moduly vytvořit reverzního proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="00cbd-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00cbd-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="00cbd-107">Prerequisites</span></span>

1. <span data-ttu-id="00cbd-108">Serveru se systémem CentOS 7 a standardní uživatelský účet s oprávněním sudo</span><span class="sxs-lookup"><span data-stu-id="00cbd-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="00cbd-109">Aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="00cbd-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="00cbd-110">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="00cbd-110">Publish the app</span></span>

<span data-ttu-id="00cbd-111">Publikování aplikace jako [samostatná nasazení](/dotnet/core/deploying/#self-contained-deployments-scd) v rámci konfigurace verze pro modul runtime CentOS 7 (`centos.7-x64`).</span><span class="sxs-lookup"><span data-stu-id="00cbd-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="00cbd-112">Zkopírujte obsah *bin/Release/netcoreapp2.0/centos.7-x64/publish* složky na serveru pomocí spojovací bod služby, FTP nebo jiné metody přenosu.</span><span class="sxs-lookup"><span data-stu-id="00cbd-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="00cbd-113">V části nasazení produkční scénář průběžnou integraci pracovní postup funguje publikování aplikace a kopírování prostředků na server.</span><span class="sxs-lookup"><span data-stu-id="00cbd-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="00cbd-114">Konfigurace proxy serveru</span><span class="sxs-lookup"><span data-stu-id="00cbd-114">Configure a proxy server</span></span>

<span data-ttu-id="00cbd-115">Reverzní proxy server je běžné instalační program pro obsluhující dynamické webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="00cbd-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="00cbd-116">Reverzní proxy server ukončí požadavek HTTP a předává do aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="00cbd-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="00cbd-117">Proxy server je takovou, která předá požadavky klientů na jiný server místo splnění sám sebe.</span><span class="sxs-lookup"><span data-stu-id="00cbd-117">A proxy server is one which forwards client requests to another server instead of fulfilling them itself.</span></span> <span data-ttu-id="00cbd-118">Reverzní proxy server předává dlouhodobý cíl, obvykle jménem libovolného klientů.</span><span class="sxs-lookup"><span data-stu-id="00cbd-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="00cbd-119">V této příručce je Apache nakonfigurovaný jako reverzní proxy server spuštěn na stejném serveru, aby Kestrel obsluhuje aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="00cbd-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

### <a name="install-apache"></a><span data-ttu-id="00cbd-120">Nainstalujte Apache</span><span class="sxs-lookup"><span data-stu-id="00cbd-120">Install Apache</span></span>

<span data-ttu-id="00cbd-121">CentOS balíčky aktualizací pro jejich nejnovější stabilní verze:</span><span class="sxs-lookup"><span data-stu-id="00cbd-121">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="00cbd-122">Instalace webového serveru Apache na CentOS s jedním `yum` příkaz:</span><span class="sxs-lookup"><span data-stu-id="00cbd-122">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="00cbd-123">Ukázka výstupu po spuštění příkazu:</span><span class="sxs-lookup"><span data-stu-id="00cbd-123">Sample output after running the command:</span></span>

```bash
Downloading packages:
httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

Installed:
httpd.x86_64 0:2.4.6-40.el7.centos.4

Complete!
```

> [!NOTE]
> <span data-ttu-id="00cbd-124">V tomto příkladu výstupu odráží httpd.86_64 vzhledem k tomu, že verze CentOS 7 je 64bitový.</span><span class="sxs-lookup"><span data-stu-id="00cbd-124">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="00cbd-125">Pokud chcete ověřit, kde je nainstalován Apache, spusťte `whereis httpd` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="00cbd-125">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="00cbd-126">Konfigurace Apache pro reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="00cbd-126">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="00cbd-127">Konfigurační soubory pro Apache se nacházejí v rámci `/etc/httpd/conf.d/` adresáře.</span><span class="sxs-lookup"><span data-stu-id="00cbd-127">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="00cbd-128">Všechny soubory s *.conf* rozšíření jsou zpracovávány v abecedním pořadí kromě souborů konfigurace modulu v `/etc/httpd/conf.modules.d/`, která obsahuje všechny konfigurační soubory nezbytné k načtení moduly.</span><span class="sxs-lookup"><span data-stu-id="00cbd-128">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="00cbd-129">Vytvoření konfiguračního souboru pro aplikaci s názvem `hellomvc.conf`:</span><span class="sxs-lookup"><span data-stu-id="00cbd-129">Create a configuration file for the app named `hellomvc.conf`:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="00cbd-130">**VirtualHost** uzlu může vyskytovat více než jednou. v jedné nebo více souborů na serveru.</span><span class="sxs-lookup"><span data-stu-id="00cbd-130">The **VirtualHost** node can appear multiple times in one or more files on a server.</span></span> <span data-ttu-id="00cbd-131">**VirtualHost** je nastaven tak, aby naslouchala na všechny IP adresy pomocí portu 80.</span><span class="sxs-lookup"><span data-stu-id="00cbd-131">**VirtualHost** is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="00cbd-132">Následující dva řádky jsou nastavené na požadavky na proxy serveru v kořenovém adresáři na server v hodnotě 127.0.0.1 na port 5000.</span><span class="sxs-lookup"><span data-stu-id="00cbd-132">The next two lines are set to proxy requests at the root to the server at 127.0.0.1 on port 5000.</span></span> <span data-ttu-id="00cbd-133">Pro obousměrnou komunikaci *ProxyPass* a *ProxyPassReverse* jsou povinné.</span><span class="sxs-lookup"><span data-stu-id="00cbd-133">For bi-directional communication, *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="00cbd-134">Můžete konfigurovat protokolování pro jednotlivé **VirtualHost** pomocí **protokolu chyb** a **CustomLog** direktivy.</span><span class="sxs-lookup"><span data-stu-id="00cbd-134">Logging can be configured per **VirtualHost** using **ErrorLog** and **CustomLog** directives.</span></span> <span data-ttu-id="00cbd-135">**Protokolu chyb** je umístění, kam server protokoluje chyby, a **CustomLog** nastaví název souboru a formát souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="00cbd-135">**ErrorLog** is the location where the server logs errors, and **CustomLog** sets the filename and format of log file.</span></span> <span data-ttu-id="00cbd-136">V takovém případě je kde požadavku je do něj protokolují informace.</span><span class="sxs-lookup"><span data-stu-id="00cbd-136">In this case, this is where request information is logged.</span></span> <span data-ttu-id="00cbd-137">Je jeden řádek pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="00cbd-137">There is one line for each request.</span></span>

<span data-ttu-id="00cbd-138">Uložte tento soubor a otestovat konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="00cbd-138">Save the file and test the configuration.</span></span> <span data-ttu-id="00cbd-139">Pokud vše projde, odpověď by měla být `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="00cbd-139">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="00cbd-140">Restartujte Apache:</span><span class="sxs-lookup"><span data-stu-id="00cbd-140">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="00cbd-141">Monitorování aplikace</span><span class="sxs-lookup"><span data-stu-id="00cbd-141">Monitoring the app</span></span>

<span data-ttu-id="00cbd-142">Apache je teď instalace předávat požadavky na `http://localhost:80` systémem Kestrel v aplikaci ASP.NET Core `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="00cbd-142">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="00cbd-143">Apache není však nastavit ke správě procesu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="00cbd-143">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="00cbd-144">Použití *systemd* a vytvořte soubor služby spuštění a sledování základní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="00cbd-144">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="00cbd-145">*systemd* je init systém, který poskytuje mnoho výkonné funkce pro spouštění, zastavování a Správa procesů.</span><span class="sxs-lookup"><span data-stu-id="00cbd-145">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="00cbd-146">Vytvoření souboru služby</span><span class="sxs-lookup"><span data-stu-id="00cbd-146">Create the service file</span></span>

<span data-ttu-id="00cbd-147">Vytvořte soubor definice služby:</span><span class="sxs-lookup"><span data-stu-id="00cbd-147">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="00cbd-148">Soubor službu příkladu pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="00cbd-148">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if dotnet service crashes
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="00cbd-149">**Uživatel** &mdash; Pokud uživatel *apache* nepoužívá konfigurace, musí být uživatel nejprve a pro soubory zadané správné vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="00cbd-149">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="00cbd-150">Uložte tento soubor a povolení služby:</span><span class="sxs-lookup"><span data-stu-id="00cbd-150">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="00cbd-151">Spusťte službu a ověřte, zda je spuštěna:</span><span class="sxs-lookup"><span data-stu-id="00cbd-151">Start the service and verify that it's running:</span></span>

```bash
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="00cbd-152">S reverzní proxy server nakonfigurovat a spravovat prostřednictvím Kestrel *systemd*, webové aplikace je plně nakonfigurovaná a je přístupný z prohlížeče v místním počítači v `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="00cbd-152">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="00cbd-153">Probíhá kontrola hlavičky odpovědi **Server** Hlavička uvádí, že je aplikace ASP.NET Core poskytovaný Kestrel:</span><span class="sxs-lookup"><span data-stu-id="00cbd-153">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="00cbd-154">Prohlížení protokolů</span><span class="sxs-lookup"><span data-stu-id="00cbd-154">Viewing logs</span></span>

<span data-ttu-id="00cbd-155">Od webové aplikace pomocí Kestrel je spravovat pomocí *systemd*, procesy a událostí jsou zaznamenány do centralizované deníku.</span><span class="sxs-lookup"><span data-stu-id="00cbd-155">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="00cbd-156">Ale tento deník obsahuje položky pro všechny služby a spravuje procesy *systemd*.</span><span class="sxs-lookup"><span data-stu-id="00cbd-156">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="00cbd-157">Chcete-li zobrazit `kestrel-hellomvc.service`-konkrétní položky, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="00cbd-157">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="00cbd-158">Pro filtrování podle času, zadejte čas možnosti pomocí příkazu.</span><span class="sxs-lookup"><span data-stu-id="00cbd-158">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="00cbd-159">Například použít `--since today` filtrovat aktuální den nebo `--until 1 hour ago` zobrazíte položky do předchozí hodiny.</span><span class="sxs-lookup"><span data-stu-id="00cbd-159">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="00cbd-160">Další informace najdete v tématu [man stránky pro journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="00cbd-160">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="00cbd-161">Zabezpečení aplikací</span><span class="sxs-lookup"><span data-stu-id="00cbd-161">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="00cbd-162">Konfigurace brány firewall</span><span class="sxs-lookup"><span data-stu-id="00cbd-162">Configure firewall</span></span>

<span data-ttu-id="00cbd-163">*Firewalld* je dynamická démon ke správě brány firewall s podporou zón sítě.</span><span class="sxs-lookup"><span data-stu-id="00cbd-163">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="00cbd-164">Porty a filtrování paketů můžete dál spravovat přes iptables.</span><span class="sxs-lookup"><span data-stu-id="00cbd-164">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="00cbd-165">*Firewalld* by měly být nainstalovány ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="00cbd-165">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="00cbd-166">`yum`slouží k instalaci balíčku nebo ověřte, zda že je nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="00cbd-166">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="00cbd-167">Použití `firewalld` k otevření pouze portů, které jsou potřebné pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="00cbd-167">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="00cbd-168">V takovém případě je to port 80 a 443 jsou používány.</span><span class="sxs-lookup"><span data-stu-id="00cbd-168">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="00cbd-169">Následující příkazy trvale nastavte porty 80 a 443 otevřete:</span><span class="sxs-lookup"><span data-stu-id="00cbd-169">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="00cbd-170">Znovu načte nastavení brány firewall.</span><span class="sxs-lookup"><span data-stu-id="00cbd-170">Reload the firewall settings.</span></span> <span data-ttu-id="00cbd-171">Zkontrolujte dostupné služby a porty ve výchozí zóně.</span><span class="sxs-lookup"><span data-stu-id="00cbd-171">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="00cbd-172">Možnosti jsou k dispozici zkontrolováním `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="00cbd-172">Options are available by inspecting `firewall-cmd -h`.</span></span>

```bash 
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

```bash
public (default, active)
interfaces: eth0
sources: 
services: dhcpv6-client
ports: 443/tcp 80/tcp
masquerade: no
forward-ports: 
icmp-blocks: 
rich rules: 
```

### <a name="ssl-configuration"></a><span data-ttu-id="00cbd-173">Konfigurace protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="00cbd-173">SSL configuration</span></span>

<span data-ttu-id="00cbd-174">Ke konfiguraci Apache pro protokol SSL, *mod_ssl* modul se používá.</span><span class="sxs-lookup"><span data-stu-id="00cbd-174">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="00cbd-175">Když *httpd* byl nainstalován modul, *mod_ssl* také nainstalován modul.</span><span class="sxs-lookup"><span data-stu-id="00cbd-175">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="00cbd-176">Pokud nebyla nainstalovaná, použijte `yum` tím ho přidáte do konfigurace.</span><span class="sxs-lookup"><span data-stu-id="00cbd-176">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="00cbd-177">Pokud chcete vynutit SSL, nainstalujte `mod_rewrite` modulu, chcete-li povolit přepisování adres URL:</span><span class="sxs-lookup"><span data-stu-id="00cbd-177">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="00cbd-178">Změnit *hellomvc.conf* souboru povolit přepisování adres URL a zabezpečenou komunikaci na portu 443:</span><span class="sxs-lookup"><span data-stu-id="00cbd-178">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> <span data-ttu-id="00cbd-179">Tento příklad používá certifikát, místně generované.</span><span class="sxs-lookup"><span data-stu-id="00cbd-179">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="00cbd-180">**SSLCertificateFile** by měl být soubor primární certifikát pro název domény.</span><span class="sxs-lookup"><span data-stu-id="00cbd-180">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="00cbd-181">**SSLCertificateKeyFile** by měl být soubor klíče vygenerován při vytváření zástupce oddělení služeb zákazníkům.</span><span class="sxs-lookup"><span data-stu-id="00cbd-181">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="00cbd-182">**SSLCertificateChainFile** by měla být soubor zprostředkující certifikátu (pokud existuje), byl zadán certifikační autoritou.</span><span class="sxs-lookup"><span data-stu-id="00cbd-182">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="00cbd-183">Uložte tento soubor a otestovat konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="00cbd-183">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="00cbd-184">Restartujte Apache:</span><span class="sxs-lookup"><span data-stu-id="00cbd-184">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="00cbd-185">Další návrhy Apache</span><span class="sxs-lookup"><span data-stu-id="00cbd-185">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="00cbd-186">Další záhlaví</span><span class="sxs-lookup"><span data-stu-id="00cbd-186">Additional headers</span></span>

<span data-ttu-id="00cbd-187">Chcete-li zabezpečení před útoky se zlými úmysly, jsou několik hlavičky, které by měl být změnit nebo přidat.</span><span class="sxs-lookup"><span data-stu-id="00cbd-187">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="00cbd-188">Ujistěte se, že `mod_headers` je modul nainstalován:</span><span class="sxs-lookup"><span data-stu-id="00cbd-188">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="00cbd-189">Apache zabezpečení před útoky útoků typu clickjacking</span><span class="sxs-lookup"><span data-stu-id="00cbd-189">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="00cbd-190">[Útoků typu Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), také známé jako *uživatelského rozhraní zjednávání nápravy útoku*, je napadením se zlými úmysly, kde je oklame kliknutím na odkaz nebo tlačítko na stránce jiné než aktuálně navštívený, aby návštěvník webu.</span><span class="sxs-lookup"><span data-stu-id="00cbd-190">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="00cbd-191">Použití `X-FRAME-OPTIONS` k zabezpečení webu.</span><span class="sxs-lookup"><span data-stu-id="00cbd-191">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="00cbd-192">Upravit *httpd.conf* souboru:</span><span class="sxs-lookup"><span data-stu-id="00cbd-192">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="00cbd-193">Přidejte řádek `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="00cbd-193">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="00cbd-194">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="00cbd-194">Save the file.</span></span> <span data-ttu-id="00cbd-195">Restartujte Apache.</span><span class="sxs-lookup"><span data-stu-id="00cbd-195">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="00cbd-196">Sledování toku dat typ MIME</span><span class="sxs-lookup"><span data-stu-id="00cbd-196">MIME-type sniffing</span></span>

<span data-ttu-id="00cbd-197">`X-Content-Type-Options` Záhlaví brání aplikaci Internet Explorer z *sledování toku dat MIME* (určení souboru `Content-Type` z obsahu souboru).</span><span class="sxs-lookup"><span data-stu-id="00cbd-197">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determing a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="00cbd-198">Pokud je server nastaví `Content-Type` hlavičky k `text/html` s `nosniff` sadu možností, Internet Explorer vykreslí obsah jako `text/html` bez ohledu na jeho obsahu.</span><span class="sxs-lookup"><span data-stu-id="00cbd-198">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="00cbd-199">Upravit *httpd.conf* souboru:</span><span class="sxs-lookup"><span data-stu-id="00cbd-199">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="00cbd-200">Přidejte řádek `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="00cbd-200">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="00cbd-201">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="00cbd-201">Save the file.</span></span> <span data-ttu-id="00cbd-202">Restartujte Apache.</span><span class="sxs-lookup"><span data-stu-id="00cbd-202">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="00cbd-203">Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="00cbd-203">Load Balancing</span></span> 

<span data-ttu-id="00cbd-204">Tento příklad ukazuje, jak nainstalovat a nakonfigurovat Apache na CentOS 7 a Kestrel na stejném počítači instance.</span><span class="sxs-lookup"><span data-stu-id="00cbd-204">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="00cbd-205">Chcete-li nemá jediný bod selhání; pomocí *mod_proxy_balancer* a úprava **VirtualHost** by umožnilo pro správu několik instancí webové aplikace za proxy serverem Apache.</span><span class="sxs-lookup"><span data-stu-id="00cbd-205">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing mutliple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="00cbd-206">V konfiguračním souboru vidíte níže, další instanci `hellomvc` aplikace je ke spuštění na portu 5001 instalace.</span><span class="sxs-lookup"><span data-stu-id="00cbd-206">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="00cbd-207">*Proxy* části nastavena konfigurace služby Vyrovnávání se dva členy pro vyrovnávání zatížení *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="00cbd-207">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPass / balancer://mycluster/ 

    ProxyPassReverse / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5001/

    <Proxy balancer://mycluster>
        BalancerMember http://127.0.0.1:5000
        BalancerMember http://127.0.0.1:5001 
        ProxySet lbmethod=byrequests
    </Proxy>

    <Location />
        SetHandler balancer
    </Location>
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="00cbd-208">Omezení přenosové rychlosti</span><span class="sxs-lookup"><span data-stu-id="00cbd-208">Rate Limits</span></span>
<span data-ttu-id="00cbd-209">Pomocí *mod_ratelimit*, který je součástí *httpd* modulu, šířku pásma klientů může být omezen:</span><span class="sxs-lookup"><span data-stu-id="00cbd-209">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="00cbd-210">Příklad souboru omezení šířky pásma jako 600 KB/s v části umístění kořenového adresáře:</span><span class="sxs-lookup"><span data-stu-id="00cbd-210">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
