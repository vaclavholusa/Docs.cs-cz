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
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a>Nastavení hostitelského prostředí pro ASP.NET Core v systému Linux s Apache a nasazení do ní

Podle [Shayne Boyer](https://github.com/spboyer)

Apache je velmi oblíbených server HTTP a může být nakonfigurován jako proxy a přesměrování provozu HTTP podobná nginx. V této příručce jsme se dozvíte, jak nastavit Apache na CentOS 7 a použít jako reverzní proxy server při Vítejte příchozí připojení a přesměruje je to ASP.NET Core aplikace běžící v Kestrel. Pro tento účel, budeme používat *mod_proxy* rozšíření a další související Apache moduly.

## <a name="prerequisites"></a>Požadavky

1. Serveru se systémem CentOS 7 a standardní uživatelský účet s oprávněním sudo.
2. Existující aplikace ASP.NET Core. 

## <a name="publish-your-application"></a>Publikování aplikace

Spustit `dotnet publish -c Release` z vývojového prostředí pro zabalení aplikace do samostatný adresář, který můžete spustit na serveru. Publikovaná aplikace musí pak zkopíruje na serveru pomocí spojovací bod služby, FTP nebo jiné metody přenosu. 

> [!NOTE]
> V části nasazení produkční scénář průběžnou integraci pracovní postup funguje publikování aplikace a kopírování prostředků na server. 

## <a name="configure-a-proxy-server"></a>Konfigurace proxy serveru

Reverzní proxy server je běžné instalační program pro obsluhující dynamické webové aplikace. Reverzní proxy server ukončí požadavek HTTP a předává do aplikace ASP.NET.

Proxy server je takovou, která předá požadavky klientů na jiný server místo splnění sám sebe. Reverzní proxy server předává dlouhodobý cíl, obvykle jménem libovolného klientů. V této příručce Apache je konfigurován jako reverzní proxy server spuštěn na stejném serveru, aby Kestrel obsluhuje aplikace ASP.NET Core. 

Každá část aplikace může existovat v samostatné fyzické počítače, Docker kontejnery nebo kombinaci konfigurace v závislosti na architektuře požadavky nebo omezení.

### <a name="install-apache"></a>Nainstalujte Apache

Instalaci webového serveru Apache na CentOS je jeden příkaz, ale první můžeme aktualizovat naše balíčky.

```bash
    sudo yum update -y
```

Tím se zajistí, že všechny nainstalované balíčky jsou aktualizovány jejich nejnovější verzi. Instalace pomocí Apache`yum`

```bash
    sudo yum -y install httpd mod_ssl
```

Výstup by měl odrážet podobný následujícímu.

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
> V tomto příkladu výstupu odráží httpd.86_64 vzhledem k tomu, že verze CentOS 7 je 64bitový. Výstup se může lišit pro váš server. Pokud chcete ověřit, kde je nainstalován Apache, spusťte `whereis httpd` z příkazového řádku. 

### <a name="configure-apache-for-reverse-proxy"></a>Konfigurace Apache pro reverzní proxy server

Konfigurační soubory pro Apache se nacházejí v rámci `/etc/httpd/conf.d/` adresáře. Všechny soubory s **.conf** rozšíření budou zpracovány v abecedním pořadí kromě souborů konfigurace modulu v `/etc/httpd/conf.modules.d/`, která obsahuje všechny konfigurační soubory nezbytné k načtení moduly.

Vytvoření konfiguračního souboru pro vaši aplikaci, v tomto příkladu, zavoláme vám ho`hellomvc.conf`

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

*VirtualHost* uzlu, které může být více v souboru nebo v mnoha souborech na serveru, je nastaven tak, aby naslouchala na všechny IP adresy pomocí portu 80. Následující dva řádky jsou nastaveny na předat všechny žádosti přijaté v kořenovém adresáři na počítač 127.0.0.1 port 5000 a pozpátku. Existovat obousměrná komunikace, obě nastavení *ProxyPass* a *ProxyPassReverse* jsou povinné.

Můžete konfigurovat protokolování pro jednotlivé VirtualHost pomocí *protokolu chyb* a *CustomLog* direktivy. *Protokolu chyb* je místo, kde server budou protokolovat chyby a *CustomLog* nastaví název souboru a formát souboru protokolu. V našem případě toto je, kde bude do protokolu informace o požadavku. Bude jeden řádek pro každý požadavek.

Uložte soubor a otestovat konfiguraci. Pokud vše projde, odpověď by měla být `Syntax [OK]`.

```bash
    sudo service httpd configtest
```

Restartujte Apache.

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a>Monitorování aplikace

Apache je teď instalace předávat požadavky na `http://localhost:80` k aplikaci ASP.NET Core systémem Kestrel v `http://127.0.0.1:5000`.  Apache není však nastavit ke správě procesu Kestrel. Budeme používat *systemd* a vytvořte soubor služby spuštění a sledování základní webové aplikace. *systemd* je init systém, který poskytuje mnoho výkonné funkce pro spouštění, zastavování a Správa procesů. 


### <a name="create-the-service-file"></a>Vytvoření souboru služby

Vytvoření souboru definice služby 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Soubor službu příkladu pro naši aplikaci.

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
> **Uživatel** – Pokud uživatel *apache* nepoužívá konfigurace, uživatelsky definované v tomto poli musí být nejprve vytvoření a pro soubory zadané správné vlastnictví

Uložte tento soubor a povolení služby.

```bash
    systemctl enable kestrel-hellomvc.service
```

Spusťte službu a ověřte, zda je spuštěna.

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

Reverzní proxy server nakonfigurovat a spravovat prostřednictvím systemd Kestrel, webové aplikace je plně nakonfigurovaná a je přístupný z prohlížeče v místním počítači v `http://localhost`. Probíhá kontrola hlavičky odpovědi **serveru** stále zobrazuje aplikace ASP.NET Core obsluhovány pomocí Kestrel.

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Prohlížení protokolů

Vzhledem k tomu, že webové aplikace pomocí Kestrel spravované pomocí systemd, všechny události a procesy jsou protokolovány centralizované deníku. Tento deník však obsahuje všechny položky pro všechny služby a spravuje systemd procesy. Chcete-li zobrazit `kestrel-hellomvc.service` konkrétní položky, použijte následující příkaz.

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

Pro další, čas možnosti filtrování, jako `--since today`, `--until 1 hour ago` nebo kombinaci těchto může snížit množství vrácena žádná položka.

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>Zabezpečení aplikace

### <a name="configure-firewall"></a>Konfigurace brány firewall

*Firewalld* je dynamická démon ke správě brány firewall s podporou zón sítě, i když stále můžete iptables ke správě porty a filtrování paketů. Ve výchozím nastavení, by měly být nainstalovány Firewalld `yum` slouží k instalaci balíčku nebo ověření.

```bash
    sudo yum install firewalld -y
```

Pomocí `firewalld` můžete otevřít pouze porty, které jsou potřebné pro aplikaci. V takovém případě je to port 80 a 443 jsou používány. Následující příkazy trvale nastaví tyto otevřete.

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

Znovu načíst nastavení brány firewall a zkontrolujte dostupné služby a porty ve výchozí zóně. Možnosti jsou k dispozici zkontrolováním`firewall-cmd -h`

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

### <a name="ssl-configuration"></a>Konfigurace protokolu SSL

Pokud chcete konfigurovat Apache pro protokol SSL, se používá modul mod_ssl.  To nainstalovaný původně při jsme nainstalovali `httpd` modulu. Pokud byl vynechán nebo jsou v něm není nainstalovaný, použijte yum přidat do vaší konfigurace.

```bash
    sudo yum install mod_ssl
```
Pokud chcete vynutit SSL, instalace`mod_rewrite`

```bash
    sudo yum install mod_rewrite
```

`hellomvc.conf` Soubor, který byl vytvořen pro tento příklad je potřeba upravit tak, aby povolit přepsání, jakož i přidání nové **VirtualHost** části pro protokol HTTPS.

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
> Tento příklad používá místně vygenerovaný certifikát. **SSLCertificateFile** by měl být soubor primární certifikátu pro název domény. **SSLCertificateKeyFile** by měl být soubor klíče, které jsou generovány, pokud jste vytvořili oddělení služeb zákazníkům. **SSLCertificateChainFile** by měla být soubor zprostředkující certifikátu (pokud existuje), byl zadán certifikační autoritou

Uložte soubor a otestovat konfiguraci.

```bash
    sudo service httpd configtest
```

Restartujte Apache.

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Další návrhy Apache

### <a name="additional-headers"></a>Další záhlaví 
Při zabezpečování proti jsou útoky se zlými úmysly existuje několik hlavičky, které by měl být změnit nebo přidat. Ujistěte se, že `mod_headers` je modul nainstalován.

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a>Zabezpečený Apache z útoků typu clickjacking
Útoků typu Clickjacking je škodlivý technika ke shromažďování nakažené uživatel klikne na. Útoků typu Clickjacking triky postižené (návštěvníka) do kliknutím na nakažené lokality. Použití X-FRAME-OPTIONS pro zabezpečení vašeho webu.

Upravte soubor httpd.conf.

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

Přidejte řádek `Header append X-FRAME-OPTIONS "SAMEORIGIN"` a uložte soubor a pak znovu spusťte Apache.

#### <a name="mime-type-sniffing"></a>Sledování toku dat typ MIME

Tuto hlavičku brání aplikaci Internet Explorer z sledování toku dat MIME jako hlavičku dostane pokyn, nechcete přepsat typ obsahu odpovědi odpověď od deklarovaný typ obsahu. S možností nosniff Pokud server informacemi o tom, že obsah je text/html, v prohlížeči bude vykreslit ho jako text/html.

Upravte soubor httpd.conf.

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

Přidejte řádek `Header set X-Content-Type-Options "nosniff"` a uložte soubor a pak znovu spusťte Apache.

### <a name="load-balancing"></a>Vyrovnávání zatížení 

Tento příklad ukazuje, jak nainstalovat a nakonfigurovat Apache na CentOS 7 a Kestrel na stejném počítači instance.  Nicméně, aby bylo možné nemá jediný bod selhání; pomocí *mod_proxy_balancer* a úpravy VirtualHost by umožňují správu několik instancí webové aplikace za proxy serverem Apache.

```bash
    sudo yum install mod_proxy_balancer
```

V konfiguračním souboru, další instanci `hellomvc` aplikací byl nastaven na port 5001 spouštět a *Proxy* oddíl nastaven pomocí konfigurace služby Vyrovnávání se dva členy pro vyrovnávání zatížení *byrequests* .

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

### <a name="rate-limits"></a>Omezení přenosové rychlosti
Pomocí `mod_ratelimit`, který je součástí `htttpd` modulu můžete omezit šířku pásma klientů. 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
Příklad souboru omezení šířky pásma jako 600 KB/s v kořenovém umístění.

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
