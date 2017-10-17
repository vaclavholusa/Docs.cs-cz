---
title: "Hostování v systému Linux s Apache ASP.NET Core"
description: "Zjistěte, jak nastavit Apache jako reverzní proxy server na CentOS a přesměrování provozu HTTP na webovou aplikaci ASP.NET Core systémem Kestrel."
keywords: "ASP.NET Core, Apache, CentOS, reverzní Proxy, Linux, mod_proxy, httpd, hostování"
author: spboyer
ms.author: spboyer
manager: wpickett
ms.date: 10/19/2016
ms.topic: article
ms.assetid: fa9b0cb7-afb3-4361-9e7e-33afffeaca0c
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/apache-proxy
ms.openlocfilehash: 34ede2fdebe0e9516f62e39618e7adba3c8c89db
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/22/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a><span data-ttu-id="97d8d-104">Nastavení hostitelského prostředí pro ASP.NET Core v systému Linux s Apache a nasazení do ní</span><span class="sxs-lookup"><span data-stu-id="97d8d-104">Set up a hosting environment for ASP.NET Core on Linux with Apache, and deploy to it</span></span>

<span data-ttu-id="97d8d-105">Podle [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="97d8d-105">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="97d8d-106">Apache je velmi oblíbených server HTTP a může být nakonfigurován jako proxy a přesměrování provozu HTTP podobná nginx.</span><span class="sxs-lookup"><span data-stu-id="97d8d-106">Apache is a very popular HTTP server and can be configured as a proxy to redirect HTTP traffic similar to nginx.</span></span> <span data-ttu-id="97d8d-107">V této příručce jsme se dozvíte, jak nastavit Apache na CentOS 7 a použít jako reverzní proxy server při Vítejte příchozí připojení a přesměruje je to ASP.NET Core aplikace běžící v Kestrel.</span><span class="sxs-lookup"><span data-stu-id="97d8d-107">In this guide, we will learn how to set up Apache on CentOS 7 and use it as a reverse proxy to welcome incoming connections and redirect them to the ASP.NET Core application running on Kestrel.</span></span> <span data-ttu-id="97d8d-108">Pro tento účel, budeme používat *mod_proxy* rozšíření a další související Apache moduly.</span><span class="sxs-lookup"><span data-stu-id="97d8d-108">For this purpose, we will use the *mod_proxy* extension and other related Apache modules.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="97d8d-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="97d8d-109">Prerequisites</span></span>

1. <span data-ttu-id="97d8d-110">Serveru se systémem CentOS 7 a standardní uživatelský účet s oprávněním sudo.</span><span class="sxs-lookup"><span data-stu-id="97d8d-110">A server running CentOS 7, with a standard user account with sudo privilege.</span></span>
2. <span data-ttu-id="97d8d-111">Existující aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="97d8d-111">An existing ASP.NET Core application.</span></span> 

## <a name="publish-your-application"></a><span data-ttu-id="97d8d-112">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="97d8d-112">Publish your application</span></span>

<span data-ttu-id="97d8d-113">Spustit `dotnet publish -c Release` z vývojového prostředí pro zabalení aplikace do samostatný adresář, který můžete spustit na serveru.</span><span class="sxs-lookup"><span data-stu-id="97d8d-113">Run `dotnet publish -c Release` from your development environment to package your application into a self-contained directory that can run on your server.</span></span> <span data-ttu-id="97d8d-114">Publikovaná aplikace musí pak zkopíruje na serveru pomocí spojovací bod služby, FTP nebo jiné metody přenosu.</span><span class="sxs-lookup"><span data-stu-id="97d8d-114">The published application must then be copied to the server using SCP, FTP or other file transfer method.</span></span> 

> [!NOTE]
> <span data-ttu-id="97d8d-115">V části nasazení produkční scénář průběžnou integraci pracovní postup funguje publikování aplikace a kopírování prostředků na server.</span><span class="sxs-lookup"><span data-stu-id="97d8d-115">Under a production deployment scenario, a continuous integration workflow does the work of publishing the application and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="97d8d-116">Konfigurace proxy serveru</span><span class="sxs-lookup"><span data-stu-id="97d8d-116">Configure a proxy server</span></span>

<span data-ttu-id="97d8d-117">Reverzní proxy server je běžné instalační program pro obsluhující dynamické webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="97d8d-117">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="97d8d-118">Reverzní proxy server ukončí požadavek HTTP a předává do aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="97d8d-118">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET application.</span></span>

<span data-ttu-id="97d8d-119">Proxy server je takovou, která předá požadavky klientů na jiný server místo splnění sám sebe.</span><span class="sxs-lookup"><span data-stu-id="97d8d-119">A proxy server is one which forwards client requests to another server instead of fulfilling them itself.</span></span> <span data-ttu-id="97d8d-120">Reverzní proxy server předává dlouhodobý cíl, obvykle jménem libovolného klientů.</span><span class="sxs-lookup"><span data-stu-id="97d8d-120">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="97d8d-121">V této příručce Apache je konfigurován jako reverzní proxy server spuštěn na stejném serveru, aby Kestrel obsluhuje aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="97d8d-121">In this guide, Apache is being configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core application.</span></span> 

<span data-ttu-id="97d8d-122">Každá část aplikace může existovat v samostatné fyzické počítače, Docker kontejnery nebo kombinaci konfigurace v závislosti na architektuře požadavky nebo omezení.</span><span class="sxs-lookup"><span data-stu-id="97d8d-122">Each piece of the application can exist on separate physical machines, Docker containers, or a combination of configurations depending on your architectural needs or restrictions.</span></span>

### <a name="install-apache"></a><span data-ttu-id="97d8d-123">Nainstalujte Apache</span><span class="sxs-lookup"><span data-stu-id="97d8d-123">Install Apache</span></span>

<span data-ttu-id="97d8d-124">Instalaci webového serveru Apache na CentOS je jeden příkaz, ale první můžeme aktualizovat naše balíčky.</span><span class="sxs-lookup"><span data-stu-id="97d8d-124">Installing the Apache web server on CentOS is a single command, but first let's update our packages.</span></span>

```bash
    sudo yum update -y
```

<span data-ttu-id="97d8d-125">Tím se zajistí, že všechny nainstalované balíčky jsou aktualizovány jejich nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="97d8d-125">This ensures that all of the installed packages are updated to their latest version.</span></span> <span data-ttu-id="97d8d-126">Instalace pomocí Apache`yum`</span><span class="sxs-lookup"><span data-stu-id="97d8d-126">Install Apache using `yum`</span></span>

```bash
    sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="97d8d-127">Výstup by měl odrážet podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="97d8d-127">The output should reflect something similar to the following.</span></span>

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
> <span data-ttu-id="97d8d-128">V tomto příkladu výstupu odráží httpd.86_64 vzhledem k tomu, že verze CentOS 7 je 64bitový.</span><span class="sxs-lookup"><span data-stu-id="97d8d-128">In this example the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="97d8d-129">Výstup se může lišit pro váš server.</span><span class="sxs-lookup"><span data-stu-id="97d8d-129">The output may be different for your server.</span></span> <span data-ttu-id="97d8d-130">Pokud chcete ověřit, kde je nainstalován Apache, spusťte `whereis httpd` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="97d8d-130">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="97d8d-131">Konfigurace Apache pro reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="97d8d-131">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="97d8d-132">Konfigurační soubory pro Apache se nacházejí v rámci `/etc/httpd/conf.d/` adresáře.</span><span class="sxs-lookup"><span data-stu-id="97d8d-132">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="97d8d-133">Všechny soubory s **.conf** rozšíření budou zpracovány v abecedním pořadí kromě souborů konfigurace modulu v `/etc/httpd/conf.modules.d/`, která obsahuje všechny konfigurační soubory nezbytné k načtení moduly.</span><span class="sxs-lookup"><span data-stu-id="97d8d-133">Any file with the **.conf** extension will be processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="97d8d-134">Vytvoření konfiguračního souboru pro vaši aplikaci, v tomto příkladu, zavoláme vám ho`hellomvc.conf`</span><span class="sxs-lookup"><span data-stu-id="97d8d-134">Create a configuration file for your app, for this example we'll call it `hellomvc.conf`</span></span>

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

<span data-ttu-id="97d8d-135">*VirtualHost* uzlu, které může být více v souboru nebo v mnoha souborech na serveru, je nastaven tak, aby naslouchala na všechny IP adresy pomocí portu 80.</span><span class="sxs-lookup"><span data-stu-id="97d8d-135">The *VirtualHost* node, of which there can be multiple in a file or on a server in many files, is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="97d8d-136">Následující dva řádky jsou nastaveny na předat všechny žádosti přijaté v kořenovém adresáři na počítač 127.0.0.1 port 5000 a pozpátku.</span><span class="sxs-lookup"><span data-stu-id="97d8d-136">The next two lines are set to pass all requests received at the root to the machine 127.0.0.1 port 5000 and in reverse.</span></span> <span data-ttu-id="97d8d-137">Existovat obousměrná komunikace, obě nastavení *ProxyPass* a *ProxyPassReverse* jsou povinné.</span><span class="sxs-lookup"><span data-stu-id="97d8d-137">For there to be bi-directional communication, both settings *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="97d8d-138">Můžete konfigurovat protokolování pro jednotlivé VirtualHost pomocí *protokolu chyb* a *CustomLog* direktivy.</span><span class="sxs-lookup"><span data-stu-id="97d8d-138">Logging can be configured per VirtualHost using *ErrorLog* and *CustomLog* directives.</span></span> <span data-ttu-id="97d8d-139">*Protokolu chyb* je místo, kde server budou protokolovat chyby a *CustomLog* nastaví název souboru a formát souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="97d8d-139">*ErrorLog* is the location where the server will log errors and *CustomLog* sets the filename and format of log file.</span></span> <span data-ttu-id="97d8d-140">V našem případě toto je, kde bude do protokolu informace o požadavku.</span><span class="sxs-lookup"><span data-stu-id="97d8d-140">In our case this is where request information will be logged.</span></span> <span data-ttu-id="97d8d-141">Bude jeden řádek pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="97d8d-141">There will be one line for each request.</span></span>

<span data-ttu-id="97d8d-142">Uložte soubor a otestovat konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="97d8d-142">Save the file, and test the configuration.</span></span> <span data-ttu-id="97d8d-143">Pokud vše projde, odpověď by měla být `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="97d8d-143">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="97d8d-144">Restartujte Apache.</span><span class="sxs-lookup"><span data-stu-id="97d8d-144">Restart Apache.</span></span>

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a><span data-ttu-id="97d8d-145">Monitorování aplikace</span><span class="sxs-lookup"><span data-stu-id="97d8d-145">Monitoring our application</span></span>

<span data-ttu-id="97d8d-146">Apache je teď instalace předávat požadavky na `http://localhost:80` k aplikaci ASP.NET Core systémem Kestrel v `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="97d8d-146">Apache is now setup to forward requests made to `http://localhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="97d8d-147">Apache není však nastavit ke správě procesu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="97d8d-147">However, Apache is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="97d8d-148">Budeme používat *systemd* a vytvořte soubor služby spuštění a sledování základní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="97d8d-148">We will use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="97d8d-149">*systemd* je init systém, který poskytuje mnoho výkonné funkce pro spouštění, zastavování a Správa procesů.</span><span class="sxs-lookup"><span data-stu-id="97d8d-149">*systemd* is an init system that provides many powerful features for starting, stopping and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="97d8d-150">Vytvoření souboru služby</span><span class="sxs-lookup"><span data-stu-id="97d8d-150">Create the service file</span></span>

<span data-ttu-id="97d8d-151">Vytvoření souboru definice služby</span><span class="sxs-lookup"><span data-stu-id="97d8d-151">Create the service definition file</span></span> 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="97d8d-152">Soubor službu příkladu pro naši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="97d8d-152">An example service file for our application.</span></span>

```text
[Unit]
    Description=Example .NET Web API Application running on CentOS 7

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
> <span data-ttu-id="97d8d-153">**Uživatel** – Pokud uživatel *apache* nepoužívá konfigurace, uživatelsky definované v tomto poli musí být nejprve vytvoření a pro soubory zadané správné vlastnictví</span><span class="sxs-lookup"><span data-stu-id="97d8d-153">**User** -- If the user *apache* is not used by your configuration, the user defined here must be created first and given proper ownership for files</span></span>

<span data-ttu-id="97d8d-154">Uložte tento soubor a povolení služby.</span><span class="sxs-lookup"><span data-stu-id="97d8d-154">Save the file and enable the service.</span></span>

```bash
    systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="97d8d-155">Spusťte službu a ověřte, zda je spuštěna.</span><span class="sxs-lookup"><span data-stu-id="97d8d-155">Start the service and verify that it is running.</span></span>

```
    systemctl start kestrel-hellomvc.service
    systemctl status kestrel-hellomvc.service

    ● kestrel-hellomvc.service - Example .NET Web API Application running on CentOS 7
        Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
        Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
    Main PID: 9021 (dotnet)
        CGroup: /system.slice/kestrel-hellomvc.service
                └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="97d8d-156">Reverzní proxy server nakonfigurovat a spravovat prostřednictvím systemd Kestrel, webové aplikace je plně nakonfigurovaná a je přístupný z prohlížeče v místním počítači v `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="97d8d-156">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="97d8d-157">Probíhá kontrola hlavičky odpovědi **serveru** stále zobrazuje aplikace ASP.NET Core obsluhovány pomocí Kestrel.</span><span class="sxs-lookup"><span data-stu-id="97d8d-157">Inspecting the response headers, the **Server** still shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="97d8d-158">Prohlížení protokolů</span><span class="sxs-lookup"><span data-stu-id="97d8d-158">Viewing logs</span></span>

<span data-ttu-id="97d8d-159">Vzhledem k tomu, že webové aplikace pomocí Kestrel spravované pomocí systemd, všechny události a procesy jsou protokolovány centralizované deníku.</span><span class="sxs-lookup"><span data-stu-id="97d8d-159">Since the web application using Kestrel is managed using systemd, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="97d8d-160">Tento deník však obsahuje všechny položky pro všechny služby a spravuje systemd procesy.</span><span class="sxs-lookup"><span data-stu-id="97d8d-160">However, this journal includes all entries for all services and processes managed by systemd.</span></span> <span data-ttu-id="97d8d-161">Chcete-li zobrazit `kestrel-hellomvc.service` konkrétní položky, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="97d8d-161">To view the `kestrel-hellomvc.service` specific items, use the following command.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="97d8d-162">Pro další, čas možnosti filtrování, jako `--since today`, `--until 1 hour ago` nebo kombinaci těchto může snížit množství vrácena žádná položka.</span><span class="sxs-lookup"><span data-stu-id="97d8d-162">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="97d8d-163">Zabezpečení aplikace</span><span class="sxs-lookup"><span data-stu-id="97d8d-163">Securing our application</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="97d8d-164">Konfigurace brány firewall</span><span class="sxs-lookup"><span data-stu-id="97d8d-164">Configure firewall</span></span>

<span data-ttu-id="97d8d-165">*Firewalld* je dynamická démon ke správě brány firewall s podporou zón sítě, i když stále můžete iptables ke správě porty a filtrování paketů.</span><span class="sxs-lookup"><span data-stu-id="97d8d-165">*Firewalld* is a dynamic daemon to manage firewall with support for network zones, although you can still use iptables to manage ports and packet filtering.</span></span> <span data-ttu-id="97d8d-166">Ve výchozím nastavení, by měly být nainstalovány Firewalld `yum` slouží k instalaci balíčku nebo ověření.</span><span class="sxs-lookup"><span data-stu-id="97d8d-166">Firewalld should be installed by default, `yum` can be used to install the package or verify.</span></span>

```bash
    sudo yum install firewalld -y
```

<span data-ttu-id="97d8d-167">Pomocí `firewalld` můžete otevřít pouze porty, které jsou potřebné pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="97d8d-167">Using `firewalld` you can open only the ports needed for the application.</span></span> <span data-ttu-id="97d8d-168">V takovém případě je to port 80 a 443 jsou používány.</span><span class="sxs-lookup"><span data-stu-id="97d8d-168">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="97d8d-169">Následující příkazy trvale nastaví tyto otevřete.</span><span class="sxs-lookup"><span data-stu-id="97d8d-169">The following commands permanently sets these to open.</span></span>

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="97d8d-170">Znovu načíst nastavení brány firewall a zkontrolujte dostupné služby a porty ve výchozí zóně.</span><span class="sxs-lookup"><span data-stu-id="97d8d-170">Reload the firewall settings, and check the available services and ports in the default zone.</span></span> <span data-ttu-id="97d8d-171">Možnosti jsou k dispozici zkontrolováním`firewall-cmd -h`</span><span class="sxs-lookup"><span data-stu-id="97d8d-171">Options are available by inspecting `firewall-cmd -h`</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="97d8d-172">Konfigurace protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="97d8d-172">SSL configuration</span></span>

<span data-ttu-id="97d8d-173">Pokud chcete konfigurovat Apache pro protokol SSL, se používá modul mod_ssl.</span><span class="sxs-lookup"><span data-stu-id="97d8d-173">To configure Apache for SSL, the mod_ssl module is used.</span></span>  <span data-ttu-id="97d8d-174">To nainstalovaný původně při jsme nainstalovali `httpd` modulu.</span><span class="sxs-lookup"><span data-stu-id="97d8d-174">This was installed initially when we installed the `httpd` module.</span></span> <span data-ttu-id="97d8d-175">Pokud byl vynechán nebo jsou v něm není nainstalovaný, použijte yum přidat do vaší konfigurace.</span><span class="sxs-lookup"><span data-stu-id="97d8d-175">If it was missed or not installed, use yum to add it to your configuration.</span></span>

```bash
    sudo yum install mod_ssl
```
<span data-ttu-id="97d8d-176">Pokud chcete vynutit SSL, instalace`mod_rewrite`</span><span class="sxs-lookup"><span data-stu-id="97d8d-176">To enforce SSL, install `mod_rewrite`</span></span>

```bash
    sudo yum install mod_rewrite
```

<span data-ttu-id="97d8d-177">`hellomvc.conf` Soubor, který byl vytvořen pro tento příklad je potřeba upravit tak, aby povolit přepsání, jakož i přidání nové **VirtualHost** části pro protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="97d8d-177">The `hellomvc.conf` file that was created for this example needs to be modified to enable the rewrite as well as adding the new **VirtualHost** section for HTTPS.</span></span>

```text
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
> <span data-ttu-id="97d8d-178">Tento příklad používá místně vygenerovaný certifikát.</span><span class="sxs-lookup"><span data-stu-id="97d8d-178">This example is using a locally generated certificate.</span></span> <span data-ttu-id="97d8d-179">**SSLCertificateFile** by měl být soubor primární certifikátu pro název domény.</span><span class="sxs-lookup"><span data-stu-id="97d8d-179">**SSLCertificateFile** should be your primary certificate file for your domain name.</span></span> <span data-ttu-id="97d8d-180">**SSLCertificateKeyFile** by měl být soubor klíče, které jsou generovány, pokud jste vytvořili oddělení služeb zákazníkům.</span><span class="sxs-lookup"><span data-stu-id="97d8d-180">**SSLCertificateKeyFile** should be the key file generated when you created the CSR.</span></span> <span data-ttu-id="97d8d-181">**SSLCertificateChainFile** by měla být soubor zprostředkující certifikátu (pokud existuje), byl zadán certifikační autoritou</span><span class="sxs-lookup"><span data-stu-id="97d8d-181">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by your certificate authority</span></span>

<span data-ttu-id="97d8d-182">Uložte soubor a otestovat konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="97d8d-182">Save the file, and test the configuration.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="97d8d-183">Restartujte Apache.</span><span class="sxs-lookup"><span data-stu-id="97d8d-183">Restart Apache.</span></span>

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="97d8d-184">Další návrhy Apache</span><span class="sxs-lookup"><span data-stu-id="97d8d-184">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="97d8d-185">Další záhlaví</span><span class="sxs-lookup"><span data-stu-id="97d8d-185">Additional Headers</span></span> 
<span data-ttu-id="97d8d-186">Při zabezpečování proti jsou útoky se zlými úmysly existuje několik hlavičky, které by měl být změnit nebo přidat.</span><span class="sxs-lookup"><span data-stu-id="97d8d-186">In order to secure against malicious attacks there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="97d8d-187">Ujistěte se, že `mod_headers` je modul nainstalován.</span><span class="sxs-lookup"><span data-stu-id="97d8d-187">Ensure that the `mod_headers` module is installed.</span></span>

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a><span data-ttu-id="97d8d-188">Zabezpečený Apache z útoků typu clickjacking</span><span class="sxs-lookup"><span data-stu-id="97d8d-188">Secure Apache from clickjacking</span></span>
<span data-ttu-id="97d8d-189">Útoků typu Clickjacking je škodlivý technika ke shromažďování nakažené uživatel klikne na.</span><span class="sxs-lookup"><span data-stu-id="97d8d-189">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="97d8d-190">Útoků typu Clickjacking triky postižené (návštěvníka) do kliknutím na nakažené lokality.</span><span class="sxs-lookup"><span data-stu-id="97d8d-190">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="97d8d-191">Použití X-FRAME-OPTIONS pro zabezpečení vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="97d8d-191">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="97d8d-192">Upravte soubor httpd.conf.</span><span class="sxs-lookup"><span data-stu-id="97d8d-192">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="97d8d-193">Přidejte řádek `Header append X-FRAME-OPTIONS "SAMEORIGIN"` a uložte soubor a pak znovu spusťte Apache.</span><span class="sxs-lookup"><span data-stu-id="97d8d-193">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"` and save the file, then restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="97d8d-194">Sledování toku dat typ MIME</span><span class="sxs-lookup"><span data-stu-id="97d8d-194">MIME-type sniffing</span></span>

<span data-ttu-id="97d8d-195">Tuto hlavičku brání aplikaci Internet Explorer z sledování toku dat MIME jako hlavičku dostane pokyn, nechcete přepsat typ obsahu odpovědi odpověď od deklarovaný typ obsahu.</span><span class="sxs-lookup"><span data-stu-id="97d8d-195">This header prevents Internet Explorer from MIME-sniffing a response away from the declared content-type as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="97d8d-196">S možností nosniff Pokud server informacemi o tom, že obsah je text/html, v prohlížeči bude vykreslit ho jako text/html.</span><span class="sxs-lookup"><span data-stu-id="97d8d-196">With the nosniff option, if the server says the content is text/html, the browser will render it as text/html.</span></span>

<span data-ttu-id="97d8d-197">Upravte soubor httpd.conf.</span><span class="sxs-lookup"><span data-stu-id="97d8d-197">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="97d8d-198">Přidejte řádek `Header set X-Content-Type-Options "nosniff"` a uložte soubor a pak znovu spusťte Apache.</span><span class="sxs-lookup"><span data-stu-id="97d8d-198">Add the line `Header set X-Content-Type-Options "nosniff"` and save the file, then restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="97d8d-199">Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="97d8d-199">Load Balancing</span></span> 

<span data-ttu-id="97d8d-200">Tento příklad ukazuje, jak nainstalovat a nakonfigurovat Apache na CentOS 7 a Kestrel na stejném počítači instance.</span><span class="sxs-lookup"><span data-stu-id="97d8d-200">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span>  <span data-ttu-id="97d8d-201">Nicméně, aby bylo možné nemá jediný bod selhání; pomocí *mod_proxy_balancer* a úpravy VirtualHost by umožňují správu několik instancí webové aplikace za proxy serverem Apache.</span><span class="sxs-lookup"><span data-stu-id="97d8d-201">However, in order to not have a single point of failure; using *mod_proxy_balancer* and modifying the VirtualHost would allow for managing mutliple instances of the web applications behind the Apache proxy server.</span></span>

```bash
    sudo yum install mod_proxy_balancer
```

<span data-ttu-id="97d8d-202">V konfiguračním souboru, další instanci `hellomvc` aplikací byl nastaven na port 5001 spouštět a *Proxy* oddíl nastaven pomocí konfigurace služby Vyrovnávání se dva členy pro vyrovnávání zatížení *byrequests* .</span><span class="sxs-lookup"><span data-stu-id="97d8d-202">In the configuration file, an additional instance of the `hellomvc` app has been setup to run on port 5001 and the *Proxy* section has been set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```text
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

### <a name="rate-limits"></a><span data-ttu-id="97d8d-203">Omezení přenosové rychlosti</span><span class="sxs-lookup"><span data-stu-id="97d8d-203">Rate Limits</span></span>
<span data-ttu-id="97d8d-204">Pomocí `mod_ratelimit`, který je součástí `htttpd` modulu můžete omezit šířku pásma klientů.</span><span class="sxs-lookup"><span data-stu-id="97d8d-204">Using `mod_ratelimit`, which is included in the `htttpd` module you can limit the amount of bandwidth of clients.</span></span> 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="97d8d-205">Příklad souboru omezení šířky pásma jako 600 KB/s v kořenovém umístění.</span><span class="sxs-lookup"><span data-stu-id="97d8d-205">The example file limits bandwidth as 600 KB/sec under the root location.</span></span>

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
