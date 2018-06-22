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
# <a name="host-aspnet-core-on-linux-with-nginx"></a>Hostování v systému Linux s Nginx ASP.NET Core

Podle [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Tato příručka vysvětluje nastavení prostředí ASP.NET Core produkční prostředí na Ubuntu 16.04 server. Tyto pokyny pravděpodobně pracovat s novější verze Ubuntu, ale pokynů nebyly testovány s novější verzí.

> [!NOTE]
> Pro Ubuntu 14.04 *supervisord* se doporučuje jako řešení pro sledování, zpracovávají Kestrel. *systemd* na Ubuntu 14.04 není dostupný. Ubuntu 14.04 pokyny najdete v tématu [předchozí verzi tohoto tématu](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).

Tato příručka:

* Umístí stávající aplikace ASP.NET Core za reverzní proxy server.
* Nastaví reverzní proxy server pro předávání požadavků na Kestrel webový server.
* Zajišťuje, že webová aplikace spuštěna při spuštění jako démon.
* Konfiguruje nástroj pro správu procesu pomohou restartování webové aplikace.

## <a name="prerequisites"></a>Požadavky

1. Přístup k serveru Ubuntu 16.04 s standardní uživatelský účet s oprávněním sudo.
1. Nainstalujte na .NET Core runtime na serveru.
   1. Přejděte [.NET Core všechny soubory ke stažení stránky](https://www.microsoft.com/net/download/all).
   1. Vyberte ze seznamu v části nejnovější modul runtime bez preview **Runtime**.
   1. Vyberte a postupujte podle pokynů pro Ubuntu, které odpovídají verzi Ubuntu server.
1. Stávající aplikace ASP.NET Core.

## <a name="publish-and-copy-over-the-app"></a>Publikování a zkopírujte přes aplikace

Konfigurace aplikace pro [nasazení závislé na framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).

Spustit [dotnet publikování](/dotnet/core/tools/dotnet-publish) z vývojového prostředí pro zabalení aplikace do adresáře (například *Koš a verze nebo&lt;target_framework_moniker&gt;/ publikovat*), můžete Spusťte na serveru:

```console
dotnet publish --configuration Release
```

Aplikace lze také publikovat jako [samostatná nasazení](/dotnet/core/deploying/#self-contained-deployments-scd) Pokud nechcete udržovat na .NET Core runtime na serveru.

Zkopírujte aplikace ASP.NET Core k serveru pomocí nástroje, který se integruje do pracovního postupu organizace (například spojovací bod služby, pomocí protokolu SFTP). Je běžné najít webové aplikace v rámci *var* directory (například *aspnetcore/var/hellomvc*).

> [!NOTE]
> V části nasazení produkční scénář průběžnou integraci pracovní postup funguje publikování aplikace a kopírování prostředků na server.

Testovací aplikace:

1. Z příkazového řádku, spusťte aplikaci: `dotnet <app_assembly>.dll`.
1. V prohlížeči přejděte na `http://<serveraddress>:<port>` k ověření, že aplikace funguje v systému Linux místně.

## <a name="configure-a-reverse-proxy-server"></a>Konfigurace serveru reverzní proxy server

Reverzní proxy server je běžné instalační program pro obsluhující dynamické webové aplikace. Reverzní proxy server ukončí požadavek HTTP a předává do aplikace ASP.NET Core.

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> Buď konfiguraci&mdash;s nebo bez reverzní proxy server&mdash;je platný a podporované konfigurace hostování pro technologii ASP.NET Core 2.0 nebo novější. Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a>Použít reverzní proxy server

Kestrel je skvělá pro obsluhující dynamický obsah z ASP.NET Core. Však nejsou webové funkce slouží jako bohaté funkce jako servery, například služby IIS, Apache nebo Nginx. Reverzní proxy server můžete přesměrovat pracovní například statický obsah obsluhuje, ukládání do mezipaměti požadavky, komprese požadavků a ukončení protokolu SSL ze serveru HTTP. Reverzní proxy server může být na vyhrazeném počítači nebo může být nasazeny společně se HTTP server.

Pro účely tohoto průvodce se používá jednu instanci Nginx. Běží na stejném serveru, spolu s HTTP server. Na základě požadavků, může být zvolen jiné instalace.

Protože požadavky jsou předávány podle reverzní proxy server, použijte [předané Middleware hlavičky](xref:host-and-deploy/proxy-load-balancer) z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) balíčku. Middleware aktualizace `Request.Scheme`pomocí `X-Forwarded-Proto` záhlaví, tak, že přesměrování identifikátory URI a jiné zásady zabezpečení pracovat správně.

Všechny součásti, které závisí na schéma, jako je například ověřování, generování odkazů, přesměrování a informace o zeměpisné poloze, musí být umístěny po vyvolání hlavičky Middleware předávat. Obecně platí by měla předávat Middleware hlavičky spustit před další middleware s výjimkou diagnostiky a zpracování middleware chyb. Toto uspořádání zajistí, že middleware spoléhat na informace předávané hlavičky spotřebovat hodnoty hlavičky pro zpracování.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Vyvolání [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metoda v `Startup.Configure` před voláním [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) nebo podobné middleware ověřování schématu. Nakonfigurujte middleware předávat `X-Forwarded-For` a `X-Forwarded-Proto` hlavičky:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Vyvolání [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metoda v `Startup.Configure` před voláním [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) a [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) nebo podobné schéma ověřování middleware. Nakonfigurujte middleware předávat `X-Forwarded-For` a `X-Forwarded-Proto` hlavičky:

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

Další konfigurace může být potřeba pro aplikace, které jsou hostovány za proxy servery a nástroje pro vyrovnávání zatížení. Další informace najdete v tématu [konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).

### <a name="install-nginx"></a>Nainstalujte Nginx

Použití `apt-get` k instalaci Nginx. Instalační program vytvoří *systemd* init skript, který spouští Nginx jako démon na spuštění systému. 

```bash
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update
apt-get install nginx
```

Ubuntu osobní balíček archivu (zásad pro posuzování projektů) je možný díky dobrovolníků a není distribuovat [nginx.org](https://nginx.org/). Další informace najdete v tématu [Nginx: binární verze: balíčky oficiální Debian/Ubuntu](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).

> [!NOTE]
> Pokud volitelné moduly Nginx, může být potřeba vytváření Nginx ze zdroje.

Vzhledem k tomu, že Nginx byla nainstalována poprvé, explicitně spusťte ji spuštěním:

```bash
sudo service nginx start
```

Ověřte, zda že prohlížeč zobrazí výchozí úvodní stránka pro Nginx. Cílová stránka je dostupný v `http://<server_IP_address>/index.nginx-debian.html`.

### <a name="configure-nginx"></a>Konfigurace Nginx

Pokud chcete konfigurovat Nginx jako reverzní proxy server pro směrování požadavků do vaší aplikace ASP.NET Core, upravte */etc/nginx/sites-available/default*. Otevřete v textovém editoru a nahraďte jeho obsah následujícím textem:

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

Pokud ne `server_name` odpovídá Nginx používá výchozí server. Pokud je definována žádná výchozí server, první server v konfiguračním souboru je výchozí server. Jako osvědčený postup přidejte konkrétní výchozí server, který vrátí stavový kód 444 v konfiguračním souboru. Příklad konfigurace serveru výchozí je:

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

S předchozím konfigurační soubor a výchozí server, Nginx přijímá veřejné přenosy na portu 80 s Hlavička hostitele `example.com` nebo `*.example.com`. Požadavky není odpovídající tito hostitelé nebude získat předávat Kestrel. Nginx předává požadavky odpovídající Kestrel v `http://localhost:5000`. V tématu [jak nginx zpracovává žádost o](https://nginx.org/docs/http/request_processing.html) Další informace.

> [!WARNING]
> Nepodařilo se určit správný [název_serveru direktiva](https://nginx.org/docs/http/server_names.html) zpřístupní v aplikaci ohrožení zabezpečení. Vazba subdomény zástupný znak (například `*.example.com`) nemá představovat toto bezpečnostní riziko, pokud řízení celého nadřazené domény (Naproti tomu `*.com`, což je snadno napadnutelný). V tématu [rfc7230 části-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Další informace.

Po vytvoření konfigurace Nginx spustit `sudo nginx -t` syntaxi konfigurační soubory. Pokud se test souboru konfigurace je úspěšné, vynutit Nginx mohla vybrat změny spuštěním `sudo nginx -s reload`.

Pro přímé spouštění aplikace na serveru:

1. Přejděte do adresáře aplikace.
1. Spustit spustitelný soubor aplikace: `./<app_executable>`.

Pokud dojde k chybě oprávnění ke změně oprávnění:

```console
chmod u+x <app_executable>
```

Pokud aplikace běží na serveru, ale přestane reagovat přes Internet, zkontrolujte brány firewall serveru a ověřte, zda je otevřený port 80. Pokud používáte virtuálního počítače s Ubuntu Azure, přidejte pravidlo skupina zabezpečení sítě (NSG), které umožní příchozí port 80 komunikaci. Jako odchozí provoz je automaticky přiděleno, pokud je povolena příchozí pravidlo, není třeba povolit pravidlo odchozí port 80.

Po dokončení testování aplikace vypněte aplikace s `Ctrl+C` na příkazovém řádku.

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

Pokud uživatel *www-data* nepoužívá konfigurace, uživatelsky definované v tomto poli musí být nejprve a pro soubory zadané správné vlastnictví.

Linux má systém souborů malá a velká písmena. Nastavení ASPNETCORE_ENVIRONMENT "Výroba" hledání konfiguračního souboru *appsettings. Production.JSON*, nikoli *appsettings.production.json*.

> [!NOTE]
> Některé hodnoty (například připojovací řetězce SQL), je nutné uvést pro zprostředkovatele konfigurace pro čtení proměnné prostředí. Použijte následující příkaz k vygenerování správně uvozený hodnoty pro použití v konfiguračním souboru:
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

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

#### <a name="change-the-nginx-response-name"></a>Změňte název Nginx odpovědi

Edit *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a>Konfigurace možností

Konfigurace serveru s další požadované moduly. Zvažte použití brány firewall webových aplikací, jako například [ModSecurity](https://www.modsecurity.org/), k posílení zabezpečení aplikace.

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

## <a name="additional-resources"></a>Další zdroje

* [Nginx: Binární verze: oficiální Debian/Ubuntu balíčky](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* [Konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer)
* [NGINX: Hlavička předané pomocí](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
