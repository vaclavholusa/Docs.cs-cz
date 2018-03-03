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
# <a name="host-aspnet-core-on-linux-with-nginx"></a>Hostování v systému Linux s Nginx ASP.NET Core

Podle [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Tato příručka vysvětluje na serveru 16.04 Ubuntu nastavení prostředí ASP.NET Core produkční prostředí.

**Poznámka:** pro Ubuntu 14.04 *supervisord* se doporučuje jako řešení pro sledování, zpracovávají Kestrel. *systemd* na Ubuntu 14.04 není dostupný. [Viz předchozí verze tohoto dokumentu](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

Tato příručka:

* Umístí stávající aplikace ASP.NET Core za reverzní proxy server.
* Nastaví reverzní proxy server pro předávání požadavků na Kestrel webový server.
* Zajišťuje, že webová aplikace spuštěna při spuštění jako démon.
* Konfiguruje nástroj pro správu procesu pomohou restartování webové aplikace.

## <a name="prerequisites"></a>Požadavky

1. Přístup k serveru 16.04 Ubuntu s standardní uživatelský účet s oprávněním sudo
1. Stávající aplikace ASP.NET Core

## <a name="copy-over-the-app"></a>Kopírování prostřednictvím aplikace

Spustit [dotnet publikování](/dotnet/core/tools/dotnet-publish) z prostředí vývojářů pro zabalení aplikace do samostatný adresář, který můžete spustit na serveru.

Kopie aplikace ASP.NET Core k serveru pomocí ať nástroj integruje do pracovního postupu organizace (například spojovací bod služby, FTP). Testování aplikace, například:

* Z příkazového řádku, spusťte `dotnet <app_assembly>.dll`.
* V prohlížeči přejděte na `http://<serveraddress>:<port>` k ověření, že aplikace funguje v systému Linux. 
 
## <a name="configure-a-reverse-proxy-server"></a>Konfigurace serveru reverzní proxy server

Reverzní proxy server je běžné instalační program pro obsluhující dynamické webové aplikace. Reverzní proxy server ukončí požadavek HTTP a předává do aplikace ASP.NET Core.

### <a name="why-use-a-reverse-proxy-server"></a>Proč používat reverzní proxy server?

Kestrel je skvělá pro obsluhující dynamický obsah z ASP.NET Core. Však nejsou webové funkce slouží jako bohaté funkce jako servery, například služby IIS, Apache nebo Nginx. Reverzní proxy server můžete přesměrovat pracovní například statický obsah obsluhuje, ukládání do mezipaměti požadavky, komprese požadavků a ukončení protokolu SSL ze serveru HTTP. Reverzní proxy server může být na vyhrazeném počítači nebo může být nasazeny společně se HTTP server.

Pro účely tohoto průvodce se používá jednu instanci Nginx. Běží na stejném serveru, spolu s HTTP server. Na základě požadavků, může být zvolen jiné instalace.

Protože požadavky jsou předávány podle reverzní proxy server, použijte hlavičky Middleware předávány z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) balíčku. Middleware aktualizace `Request.Scheme`pomocí `X-Forwarded-Proto` záhlaví, tak, že přesměrování identifikátory URI a jiné zásady zabezpečení pracovat správně.

Při použití jakéhokoli typu middleware ověřování, musíte spustit první Middleware předávaných hlavičky. Toto uspořádání zajistí, že ověřovací middleware může spotřebovávat hodnoty hlavičky a generovat správné přesměrování identifikátory URI.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Vyvolání [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metoda v `Startup.Configure` před voláním [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) nebo podobné middleware ověřování schématu:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Vyvolání [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metoda v `Startup.Configure` před voláním [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) a [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) nebo podobné schéma ověřování Middleware:

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

Pokud žádné [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) jsou určené pro middleware, jsou výchozí hlavičky předávat `None`.

### <a name="install-nginx"></a>Nainstalujte Nginx

```bash
sudo apt-get install nginx
```

> [!NOTE]
> Pokud se nainstalují volitelné moduly Nginx, může být potřeba vytváření Nginx ze zdroje.

Použití `apt-get` k instalaci Nginx. Instalační program vytvoří skript init V systému, který spouští Nginx jako démon na spuštění systému. Vzhledem k tomu, že Nginx byla nainstalována poprvé, explicitně spusťte ji spuštěním:

```bash
sudo service nginx start
```

Ověřte, zda že prohlížeč zobrazí výchozí úvodní stránka pro Nginx.

### <a name="configure-nginx"></a>Konfigurace Nginx

Pokud chcete konfigurovat Nginx jako reverzní proxy server pro směrování požadavků do vaší aplikace ASP.NET Core, upravte `/etc/nginx/sites-available/default`. Otevřete v textovém editoru a nahraďte jeho obsah následujícím textem:

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

Tento konfigurační soubor Nginx předává veřejné příchozí provoz z portu `80` na port `5000`.

Po vytvoření konfigurace Nginx spustit `sudo nginx -t` syntaxi konfigurační soubory. Pokud se test souboru konfigurace je úspěšné, vynutit Nginx mohla vybrat změny spuštěním `sudo nginx -s reload`.

## <a name="monitoring-the-app"></a>Monitorování aplikace

Server je instalační program předávat požadavky na `http://<serveraddress>:80` k aplikaci ASP.NET Core spuštěnou na Kestrel v `http://127.0.0.1:5000`. Nginx není však nastavit ke správě procesu Kestrel. *systemd* slouží k vytvoření souboru služby spuštění a sledování základní webové aplikace. *systemd* je init systém, který poskytuje mnoho výkonné funkce pro spouštění, zastavování a Správa procesů. 

### <a name="create-the-service-file"></a>Vytvoření souboru služby

Vytvořte soubor definice služby:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Následující příklad je služba soubor pro aplikaci:

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

**Poznámka:** Pokud uživatel *www-data* nepoužívá konfigurace, uživatelsky definované v tomto poli musí být nejprve a pro soubory zadané správné vlastnictví.
**Poznámka:** Linux má systém souborů malá a velká písmena. Nastavení ASPNETCORE_ENVIRONMENT "Výroba" hledání konfiguračního souboru *appsettings. Production.JSON*, nikoli *appsettings.production.json*.

Uložte tento soubor a povolení služby.

```bash
systemctl enable kestrel-hellomvc.service
```

Spusťte službu a ověřte, zda je spuštěna.

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

## <a name="securing-the-app"></a>Zabezpečení aplikací

### <a name="enable-apparmor"></a>Povolit AppArmor

Linux zabezpečení moduly (LSM) je rozhraní, které je součástí jádra Linux od Linux 2.6. LSM podporuje různé implementace modulů zabezpečení. [AppArmor](https://wiki.ubuntu.com/AppArmor) je LSM, který implementuje systém povinné řízení přístupu, který umožní uzavírání program, který má omezenou sadu prostředků. Zkontrolujte AppArmor je povolená a správně nakonfigurovaná.

### <a name="configuring-the-firewall"></a>Konfigurace brány firewall

Zavřete vypnout všechny externí porty, které nejsou používána. Nekomplikované brány firewall (ufw) poskytuje front-end pro `iptables` tím, že poskytuje rozhraní příkazového řádku pro konfiguraci brány firewall. Ověřte, že `ufw` nastaven tak, aby povolovala přenosy na všechny porty potřebné.

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

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>Změňte název Nginx odpovědi

Edit *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>Nakonfigurujte možnosti a sestavení

Knihovna PCRE je vyžadována pro regulární výrazy. Regulární výrazy se používají v direktivě umístění pro ngx_http_rewrite_module. Http_ssl_module přidává podporu protokolu HTTPS.

Zvažte použití brány firewall webových aplikací jako *ModSecurity* k posílení zabezpečení aplikace.

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

* Nepřidáte záhlaví zabezpečení přenosu Strict nebo zvolit odpovídající `max-age` Pokud bude v budoucnu zakázán protokol SSL.

Přidat */etc/nginx/proxy.conf* konfigurační soubor:

[!code-nginx[](linux-nginx/proxy.conf)]

Upravit */etc/nginx/nginx.conf* konfigurační soubor. V příkladu obsahuje oba `http` a `server` oddílů v souboru jednu konfiguraci.

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Zabezpečený Nginx z útoků typu clickjacking
Útoků typu Clickjacking je škodlivý technika ke shromažďování nakažené uživatel klikne na. Útoků typu Clickjacking triky postižené (návštěvníka) do kliknutím na nakažené lokality. Použití X-FRAME-OPTIONS k zabezpečení webu.

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
