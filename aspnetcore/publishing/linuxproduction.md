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
ms.openlocfilehash: 2c7401657486a8e5dbc8213d79dcfd5e0ec76585
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a>Nastavení hostitelského prostředí pro ASP.NET Core v systému Linux s Nginx a nasazení do ní

Podle [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Tato příručka vysvětluje na serveru 16.04 Ubuntu nastavení prostředí ASP.NET Core produkční prostředí.

**Poznámka:** pro Ubuntu 14.04, se doporučuje supervisord jako řešení pro sledování, zpracovávají Kestrel. systemd není k dispozici na Ubuntu 14.04. [Viz předchozí verze tohoto dokumentu](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

Tato příručka:

* Umístí existující aplikaci ASP.NET Core za reverzní proxy server
* Nastaví reverzního proxy serveru tak, aby předávat požadavky na webový server Kestrel
* Zajišťuje, že je webová aplikace spuštěna při spuštění jako démon
* Konfiguruje nástroj pro správu procesu pomohou restartování webové aplikace

## <a name="prerequisites"></a>Požadavky

1. Přístup k serveru 16.04 Ubuntu s standardní uživatelský účet s oprávněním sudo
2. Existující aplikace ASP.NET Core

## <a name="copy-over-your-app"></a>Zkopírujte nad vaší aplikace

Spustit `dotnet publish` z prostředí vývojářů pro zabalení aplikace do samostatný adresář, který můžete spustit na serveru.

Zkopírujte aplikace ASP.NET Core k serveru pomocí ať nástroj (spojovací bod služby, FTP, atd.) se integruje do vašeho pracovního postupu. Testování aplikace, například:
 - Z příkazového řádku, spusťte`dotnet yourapp.dll`
 - V prohlížeči přejděte na `http://<serveraddress>:<port>` k ověření, že aplikace funguje v systému Linux. 
 
## <a name="configure-a-reverse-proxy-server"></a>Konfigurace serveru reverzní proxy server

Reverzní proxy server je běžné instalační program pro obsluhující dynamické webové aplikace. Reverzní proxy server ukončí požadavek HTTP a předává do aplikace ASP.NET Core.

### <a name="why-use-a-reverse-proxy-server"></a>Proč používat reverzní proxy server?

Kestrel je skvělá pro obsluhující dynamický obsah z ASP.NET Core; však nejsou webové části slouží jako bohaté funkce jako servery jako služby IIS, Apache nebo Nginx. Reverzní proxy server můžete přesměrovat pracovní obsluhující statický obsah, ukládání do mezipaměti požadavky, komprese požadavků a ukončení protokolu SSL ze serveru HTTP. Reverzní proxy server může být na vyhrazeném počítači nebo může být nasazeny společně se HTTP server.

Pro účely tohoto průvodce se používá jednu instanci Nginx. Běží na stejném serveru, spolu s HTTP server. Podle potřeb, můžete zvolit jiné instalace.

Protože požadavky jsou předávány podle reverzní proxy server, použijte `ForwardedHeaders` middleware z `Microsoft.AspNetCore.HttpOverrides` balíčku. Tento middleware aktualizace `Request.Scheme`pomocí `X-Forwarded-Proto` záhlaví, tak, že přesměrování identifikátory URI a jiné zásady zabezpečení pracovat správně.

Při nastavování reverzní proxy server, musí middleware ověřování `UseForwardedHeaders` má spustit jako první. Toto uspořádání zajistí, že ověřovací middleware může spotřebovávat ovlivněných hodnoty a generovat správné přesměrování identifikátory URI.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Vyvolání `UseForwardedHeaders` – metoda (v `Configure` metodu *Startup.cs*) před voláním `UseAuthentication` nebo podobné middleware ověřování schématu:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Vyvolání `UseForwardedHeaders` – metoda (v `Configure` metodu *Startup.cs*) před voláním `UseIdentity` a `UseFacebookAuthentication` nebo podobné middleware ověřování schématu:

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

### <a name="install-nginx"></a>Nainstalujte Nginx

```bash
sudo apt-get install nginx
```

> [!NOTE]
> Pokud máte v plánu pro instalaci volitelné moduly Nginx, může být nutné vytvořit Nginx ze zdroje.

Použití `apt-get` k instalaci Nginx. Instalační program vytvoří skript init V systému, který spouští Nginx jako démon na spuštění systému. Vzhledem k tomu, že Nginx byla nainstalována poprvé, explicitně spusťte ji spuštěním:

```bash
sudo service nginx start
```

Ověřte, zda že prohlížeč zobrazí výchozí úvodní stránka pro Nginx.

### <a name="configure-nginx"></a>Konfigurace Nginx

Pokud chcete konfigurovat Nginx jako reverzní proxy server pro směrování požadavků na aplikace ASP.NET Core, upravte `/etc/nginx/sites-available/default`. Otevřete v textovém editoru a nahraďte jeho obsah následujícím textem:

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Tento konfigurační soubor Nginx předává veřejné příchozí provoz z portu `80` na port `5000`.

Po dokončení provádění změn konfigurace Nginx, můžete spustit `sudo nginx -t` syntaxi konfiguračních souborů. Pokud se test souboru konfigurace je úspěšné, požádejte Nginx mohla vybrat změny spuštěním `sudo nginx -s reload`.

## <a name="monitoring-our-application"></a>Monitorování aplikace

Nginx je teď instalace předávat požadavky na `http://yourhost:80` k aplikaci ASP.NET Core systémem Kestrel v `http://127.0.0.1:5000`. Nginx není však nastavit ke správě procesu Kestrel. Můžete použít *systemd* a vytvořte soubor služby spuštění a sledování základní webové aplikace. *systemd* je init systém, který poskytuje mnoho výkonné funkce pro spouštění, zastavování a Správa procesů. 

### <a name="create-the-service-file"></a>Vytvoření souboru služby

Vytvořte soubor definice služby:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Následující příklad je služba soubor pro naši aplikaci:

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

**Poznámka:** Pokud uživatel *www-data* nepoužívá konfigurace, uživatelsky definované v tomto poli musí být nejprve vytvoření a pro soubory zadané správné vlastnictví.

Uložte soubor a povolení služby.

```bash
systemctl enable kestrel-hellomvc.service
```

Spusťte službu a ověřte, zda je spuštěna.

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

Reverzní proxy server nakonfigurovat a spravovat prostřednictvím systemd Kestrel, webové aplikace je plně nakonfigurovaná a je přístupný z prohlížeče v místním počítači v `http://localhost`. Je také přístupné ze vzdáleného počítače, blokování žádné brány firewall, která může být blokování. Probíhá kontrola hlavičky odpovědi `Server` záhlaví zobrazuje aplikace ASP.NET Core obsluhovány pomocí Kestrel.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Prohlížení protokolů

Od webové aplikace pomocí Kestrel je spravovat pomocí `systemd`, všechny události a procesy, které jsou zaznamenány do centralizované deníku. Ale tento deník obsahuje všechny položky pro všechny služby a spravuje procesy `systemd`. Chcete-li zobrazit `kestrel-hellomvc.service`-konkrétní položky, použijte následující příkaz:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Pro další, čas možnosti filtrování, jako `--since today`, `--until 1 hour ago` nebo kombinaci těchto může snížit množství vrácena žádná položka.

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>Zabezpečení aplikace

### <a name="enable-apparmor"></a>Povolit AppArmor

Linux zabezpečení moduly (LSM) je rozhraní, které je součástí jádra Linux od Linux 2.6. LSM podporuje různé implementace modulů zabezpečení. [AppArmor](https://wiki.ubuntu.com/AppArmor) je LSM, který implementuje systém povinné řízení přístupu, který umožní uzavírání program, který má omezenou sadu prostředků. Zkontrolujte AppArmor je povolená a správně nakonfigurovaná.

### <a name="configuring-our-firewall"></a>Konfigurace naše brány firewall

Zavřete vypnout všechny externí porty, které nejsou používána. Nekomplikované brány firewall (ufw) poskytuje front-end pro `iptables` tím, že poskytuje rozhraní příkazového řádku pro konfiguraci brány firewall. Ověřte, že `ufw` nastaven tak, aby povolovala přenosy na všechny porty, které potřebujete.

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a>Zabezpečení Nginx

Výchozí distribuci Nginx není povolit protokol SSL. Pokud chcete povolit další funkce zabezpečení, sestavení ze zdroje.

#### <a name="download-the-source-and-install-the-build-dependencies"></a>Stáhnout zdrojovou verzi a instalaci závislostí sestavení

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>Změňte název Nginx odpovědi

Upravit *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>Nakonfigurujte možnosti a sestavení

Knihovna PCRE je vyžadována pro regulární výrazy. Regulární výrazy se používají v direktivě umístění pro ngx_http_rewrite_module. Http_ssl_module přidává podporu protokolu HTTPS.

Zvažte použití brány firewall webových aplikací jako *ModSecurity* k posílení zabezpečení vaší aplikace.

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>Konfigurace protokolu SSL

* Konfigurace serveru pro naslouchání na přenosy HTTPS na portu `443` zadáním platný certifikát vydaný důvěryhodné certifikační autority (CA).

* Posílení zabezpečení díky využití architektury některé postupy znázorněn v následujícím */etc/nginx/nginx.conf* souboru. Mezi příklady patří výběr silnější šifrování a přesměrování veškerý provoz prostřednictvím protokolu HTTP do HTTPS.

* Přidání `HTTP Strict-Transport-Security` hlavičky (HSTS) zajišťuje, všechny následné žádosti od klienta jsou jenom přes protokol HTTPS.

* Nepřidávejte záhlaví zabezpečení přenosu Strict nebo zvolit odpovídající `max-age` Pokud budete chtít v budoucnu zakázat protokol SSL.

Přidat */etc/nginx/proxy.conf* konfigurační soubor:

[!code-nginx[Main](linuxproduction/proxy.conf)]

Upravit */etc/nginx/nginx.conf* konfigurační soubor. V příkladu obsahuje oba `http` a `server` oddílů v souboru jednu konfiguraci.

[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Zabezpečený Nginx z útoků typu clickjacking
Útoků typu Clickjacking je škodlivý technika ke shromažďování nakažené uživatel klikne na. Útoků typu Clickjacking triky postižené (návštěvníka) do kliknutím na nakažené lokality. Použití X-FRAME-OPTIONS pro zabezpečení vašeho webu.

Upravit *nginx.conf* souboru:

```bash
sudo nano /etc/nginx/nginx.conf
```

Přidejte řádek `add_header X-Frame-Options "SAMEORIGIN";` a uložte soubor a pak znovu spusťte Nginx.

#### <a name="mime-type-sniffing"></a>Sledování toku dat typ MIME

Tuto hlavičku zabraňuje většina prohlížečů z sledování toku dat MIME odpověď od deklarovaný typ obsahu, jako hlavičku dostane pokyn, nechcete přepsat typ obsahu odpovědi. Pomocí `nosniff` možnost, pokud server uvádí obsah je "text/html", v prohlížeči se vykreslí jako "text/html".

Upravit *nginx.conf* souboru:

```bash
sudo nano /etc/nginx/nginx.conf
```

Přidejte řádek `add_header X-Content-Type-Options "nosniff";` a uložte soubor a pak znovu spusťte Nginx.
