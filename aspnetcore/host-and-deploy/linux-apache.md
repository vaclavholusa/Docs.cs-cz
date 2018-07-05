---
title: Hostitele ASP.NET Core v Linuxu pomocí Apache
description: Zjistěte, jak nastavit službu Apache jako reverzní proxy server na CentOS pro přesměrování přenosu dat protokolu HTTP k webové aplikaci ASP.NET Core spuštěnou v prostředí Kestrel.
author: spboyer
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
uid: host-and-deploy/linux-apache
ms.openlocfilehash: d02fbd82be37e6d67214a9a0bf5851662b577cb9
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433971"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="ae22b-103">Hostitele ASP.NET Core v Linuxu pomocí Apache</span><span class="sxs-lookup"><span data-stu-id="ae22b-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="ae22b-104">Podle [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="ae22b-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="ae22b-105">Pomocí této příručky, zjistěte, jak nastavit [Apache](https://httpd.apache.org/) jako reverzní proxy server na [CentOS 7](https://www.centos.org/) pro přesměrování přenosu dat protokolu HTTP pro webovou aplikaci ASP.NET Core využívající [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="ae22b-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="ae22b-106">[Mod_proxy rozšíření](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) a související moduly vytvořit reverzního proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="ae22b-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ae22b-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ae22b-107">Prerequisites</span></span>

1. <span data-ttu-id="ae22b-108">Server se systémem CentOS 7 pomocí standardního uživatelského účtu s oprávněními sudo.</span><span class="sxs-lookup"><span data-stu-id="ae22b-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="ae22b-109">Nainstalujte modul runtime .NET Core na serveru.</span><span class="sxs-lookup"><span data-stu-id="ae22b-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="ae22b-110">Přejděte [.NET Core všechny soubory ke stažení stránky](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="ae22b-110">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="ae22b-111">Vyberte nejnovější modul runtime – ve verzi preview ze seznamu **Runtime**.</span><span class="sxs-lookup"><span data-stu-id="ae22b-111">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="ae22b-112">Vyberte a postupujte podle pokynů pro CentOS nebo Oracle.</span><span class="sxs-lookup"><span data-stu-id="ae22b-112">Select and follow the instructions for CentOS/Oracle.</span></span>
1. <span data-ttu-id="ae22b-113">Stávající aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ae22b-113">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="ae22b-114">Publikování a zkopírujte myší na aplikaci</span><span class="sxs-lookup"><span data-stu-id="ae22b-114">Publish and copy over the app</span></span>

<span data-ttu-id="ae22b-115">Konfigurace aplikace pro [nasazení závisí na architektuře](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="ae22b-115">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="ae22b-116">Spustit [dotnet publikovat](/dotnet/core/tools/dotnet-publish) z vývojového prostředí pro balíček aplikace do adresáře (například *bin/Release/&lt;target_framework_moniker&gt;/ publish*), který můžete Spusťte na serveru:</span><span class="sxs-lookup"><span data-stu-id="ae22b-116">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="ae22b-117">Aplikace můžete také publikovat jako [samostatná nasazení](/dotnet/core/deploying/#self-contained-deployments-scd) Pokud nechcete zachovat modulu runtime .NET Core na serveru.</span><span class="sxs-lookup"><span data-stu-id="ae22b-117">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="ae22b-118">Aplikace ASP.NET Core zkopírujte na server pomocí nástroje, které se integruje do pracovního postupu organizace (třeba spojovací bod služby, SFTP).</span><span class="sxs-lookup"><span data-stu-id="ae22b-118">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="ae22b-119">Je běžné vyhledejte webové aplikace v rámci *var* adresář (třeba *aspnetcore/var/hellomvc*).</span><span class="sxs-lookup"><span data-stu-id="ae22b-119">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="ae22b-120">V případě produkčního nasazení pracovního postupu průběžné integrace funguje publikování aplikace a kopírování prostředky na server.</span><span class="sxs-lookup"><span data-stu-id="ae22b-120">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="ae22b-121">Konfigurace proxy serveru</span><span class="sxs-lookup"><span data-stu-id="ae22b-121">Configure a proxy server</span></span>

<span data-ttu-id="ae22b-122">Reverzní proxy server je společné nastavení pro poskytování dynamické webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae22b-122">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="ae22b-123">Reverzní proxy server ukončí požadavek HTTP a předá ji do aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ae22b-123">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="ae22b-124">Proxy server je znak, který se předává požadavky klientů na jiný server místo samotného plnění požadavků.</span><span class="sxs-lookup"><span data-stu-id="ae22b-124">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="ae22b-125">Reverzní proxy server předává do pevné umístění, obvykle jménem libovolného klientů.</span><span class="sxs-lookup"><span data-stu-id="ae22b-125">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="ae22b-126">V této příručce Apache nakonfigurovaný jako reverzní proxy server běží na stejném serveru, aby Kestrel obsluhuje aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ae22b-126">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="ae22b-127">Vzhledem k tomu, že žádosti jsou předávány podle reverzní proxy server, použít [předané Middleware záhlaví](xref:host-and-deploy/proxy-load-balancer) z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="ae22b-127">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="ae22b-128">Middleware aktualizace `Request.Scheme`, použije `X-Forwarded-Proto` záhlaví tak, že identifikátory URI pro přesměrování a další zásady zabezpečení pracovat správně.</span><span class="sxs-lookup"><span data-stu-id="ae22b-128">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="ae22b-129">Jakékoli součásti, která závisí na schéma, jako je například ověřování, generování odkazů, přesměrování a zeměpisná poloha, musí být umístěn po vyvolání Middleware předané záhlaví.</span><span class="sxs-lookup"><span data-stu-id="ae22b-129">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="ae22b-130">Jako obecné pravidlo by měla předávat Middleware záhlaví spustit před dalším middlewarem s výjimkou diagnostiky a middleware pro zpracování chyb.</span><span class="sxs-lookup"><span data-stu-id="ae22b-130">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="ae22b-131">Toto uspořádání zajistí, že middleware spoléhání se na informace předávané záhlaví může spotřebovat hodnoty hlavičky pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="ae22b-131">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

::: moniker range=">= aspnetcore-2.0"
> [!NOTE]
> <span data-ttu-id="ae22b-132">Buď konfiguraci&mdash;s nebo bez něj reverzní proxy server&mdash;je platný a podporované konfigurace pro hostování pro ASP.NET Core 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ae22b-132">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="ae22b-133">Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="ae22b-133">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>
::: moniker-end

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ae22b-134">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ae22b-134">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ae22b-135">Vyvolat [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metoda `Startup.Configure` před voláním [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) nebo podobné režimu middleware ověřování.</span><span class="sxs-lookup"><span data-stu-id="ae22b-135">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="ae22b-136">Nakonfigurujte middleware předávat `X-Forwarded-For` a `X-Forwarded-Proto` hlavičky:</span><span class="sxs-lookup"><span data-stu-id="ae22b-136">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ae22b-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ae22b-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ae22b-138">Vyvolat [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metoda `Startup.Configure` před voláním [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) a [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) nebo podobné schéma ověřování middleware.</span><span class="sxs-lookup"><span data-stu-id="ae22b-138">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="ae22b-139">Nakonfigurujte middleware předávat `X-Forwarded-For` a `X-Forwarded-Proto` hlavičky:</span><span class="sxs-lookup"><span data-stu-id="ae22b-139">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="ae22b-140">Pokud ne [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) jsou určené pro middleware, jsou výchozí hlavičky pro předávání `None`.</span><span class="sxs-lookup"><span data-stu-id="ae22b-140">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="ae22b-141">Další konfigurace může být nezbytný pro aplikací hostovaných za službou proxy servery a nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="ae22b-141">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="ae22b-142">Další informace najdete v tématu [konfigurace ASP.NET Core práci se servery proxy a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="ae22b-142">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="ae22b-143">Instalace Apache</span><span class="sxs-lookup"><span data-stu-id="ae22b-143">Install Apache</span></span>

<span data-ttu-id="ae22b-144">CentOS balíčky aktualizací pro jejich nejnovější stabilní verze:</span><span class="sxs-lookup"><span data-stu-id="ae22b-144">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="ae22b-145">Instalace webového serveru Apache na CentOS pomocí jediného `yum` příkaz:</span><span class="sxs-lookup"><span data-stu-id="ae22b-145">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="ae22b-146">Ukázkový výstup po spuštění příkazu:</span><span class="sxs-lookup"><span data-stu-id="ae22b-146">Sample output after running the command:</span></span>

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
> <span data-ttu-id="ae22b-147">V tomto příkladu výstupu odráží httpd.86_64 od verze CentOS 7 je 64bitový.</span><span class="sxs-lookup"><span data-stu-id="ae22b-147">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="ae22b-148">Chcete-li zkontrolovat, kde je nainstalován Apache, `whereis httpd` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="ae22b-148">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="ae22b-149">Nakonfigurovat i Apache</span><span class="sxs-lookup"><span data-stu-id="ae22b-149">Configure Apache</span></span>

<span data-ttu-id="ae22b-150">Konfigurační soubory pro Apache jsou umístěné v rámci `/etc/httpd/conf.d/` adresáře.</span><span class="sxs-lookup"><span data-stu-id="ae22b-150">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="ae22b-151">Žádný soubor s *.conf* rozšíření, jsou zpracovávána v abecedním pořadí kromě souborů konfigurace modulu v `/etc/httpd/conf.modules.d/`, která obsahuje veškeré konfigurace soubory nezbytné k načtení modulů.</span><span class="sxs-lookup"><span data-stu-id="ae22b-151">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="ae22b-152">Vytvoření konfiguračního souboru s názvem *hellomvc.conf*, pro aplikace:</span><span class="sxs-lookup"><span data-stu-id="ae22b-152">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}hellomvc-error.log
    CustomLog ${APACHE_LOG_DIR}hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="ae22b-153">`VirtualHost` Bloku se mohou objevit více než jednou, v jedné nebo více souborů na serveru.</span><span class="sxs-lookup"><span data-stu-id="ae22b-153">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="ae22b-154">V předchozím konfigurační soubor Apache přijímá veřejné provozu na portu 80.</span><span class="sxs-lookup"><span data-stu-id="ae22b-154">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="ae22b-155">Doména `www.example.com` se obsluhuje a `*.example.com` alias se překládá na stejné webové stránce.</span><span class="sxs-lookup"><span data-stu-id="ae22b-155">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="ae22b-156">Zobrazit [podporu založené na název virtuálního hostitele](https://httpd.apache.org/docs/current/vhosts/name-based.html) Další informace.</span><span class="sxs-lookup"><span data-stu-id="ae22b-156">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="ae22b-157">Požadavky jsou směrovány přes proxy server v kořenovém adresáři na portu 5000 serveru na 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="ae22b-157">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="ae22b-158">Pro obousměrnou komunikaci `ProxyPass` a `ProxyPassReverse` jsou povinné.</span><span class="sxs-lookup"><span data-stu-id="ae22b-158">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span> <span data-ttu-id="ae22b-159">Chcete-li změnit Kestrel jeho IP adresa/port, [Kestrel: konfigurace koncového bodu](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="ae22b-159">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="ae22b-160">Nepodařilo se určit správnou [ServerName směrnice](https://httpd.apache.org/docs/current/mod/core.html#servername) v **VirtualHost** bloku zpřístupňuje aplikaci tak, aby slabá místa zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="ae22b-160">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="ae22b-161">Vazby zástupný znak subdoménu (například `*.example.com`) nemá představovat toto bezpečnostní riziko, pokud řídíte celý nadřazené domény (nikoli `*.com`, což je ohrožené).</span><span class="sxs-lookup"><span data-stu-id="ae22b-161">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="ae22b-162">Zobrazit [rfc7230 části-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Další informace.</span><span class="sxs-lookup"><span data-stu-id="ae22b-162">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="ae22b-163">Protokolování lze nastavit za `VirtualHost` pomocí `ErrorLog` a `CustomLog` direktivy.</span><span class="sxs-lookup"><span data-stu-id="ae22b-163">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="ae22b-164">`ErrorLog` je umístění, kde server protokoluje chyby, a `CustomLog` nastaví název souboru a formát souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="ae22b-164">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="ae22b-165">V takovém případě je kde zaznamenané informace o žádostech.</span><span class="sxs-lookup"><span data-stu-id="ae22b-165">In this case, this is where request information is logged.</span></span> <span data-ttu-id="ae22b-166">Existuje jeden řádek pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="ae22b-166">There's one line for each request.</span></span>

<span data-ttu-id="ae22b-167">Uložte soubor a testovací konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ae22b-167">Save the file and test the configuration.</span></span> <span data-ttu-id="ae22b-168">Pokud vše projde, odpověď by měla být `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="ae22b-168">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="ae22b-169">Restartujte Apache:</span><span class="sxs-lookup"><span data-stu-id="ae22b-169">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="ae22b-170">Monitorování aplikace</span><span class="sxs-lookup"><span data-stu-id="ae22b-170">Monitoring the app</span></span>

<span data-ttu-id="ae22b-171">Apache je nyní instalačního programu předat požadavky na `http://localhost:80` pro aplikaci ASP.NET Core spuštěnou v Kestrel na `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="ae22b-171">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="ae22b-172">Apache není však nastavené ke správě procesu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ae22b-172">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="ae22b-173">Použití *systemd* a vytvořit soubor služby a začít monitorovat základní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ae22b-173">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="ae22b-174">*systemd* je init systém, který poskytuje řadu výkonných funkcí pro spouštění, zastavování a Správa procesů.</span><span class="sxs-lookup"><span data-stu-id="ae22b-174">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="ae22b-175">Vytvoření souboru služby</span><span class="sxs-lookup"><span data-stu-id="ae22b-175">Create the service file</span></span>

<span data-ttu-id="ae22b-176">Vytvoření definičního souboru služby:</span><span class="sxs-lookup"><span data-stu-id="ae22b-176">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="ae22b-177">Příklad souboru služby pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="ae22b-177">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="ae22b-178">**Uživatel** &mdash; Pokud uživatel *apache* nepoužívá konfigurace, musí nejprve vytvořit uživatele a pro soubory zadané správné vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="ae22b-178">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

> [!NOTE]
> <span data-ttu-id="ae22b-179">Některé hodnoty (například připojovací řetězce SQL) musí být uvozena pro zprostředkovatele konfigurace pro čtení proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="ae22b-179">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="ae22b-180">Použijte následující příkaz k vygenerování správně uvozený uvozovacím znakem hodnoty pro použití v konfiguračním souboru:</span><span class="sxs-lookup"><span data-stu-id="ae22b-180">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="ae22b-181">Uložte soubor a povolení služby:</span><span class="sxs-lookup"><span data-stu-id="ae22b-181">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="ae22b-182">Spusťte službu a ověřte, zda je spuštěna:</span><span class="sxs-lookup"><span data-stu-id="ae22b-182">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="ae22b-183">Reverzní proxy server nakonfigurovaný a spravované přes Kestrel *systemd*, webové aplikace plně konfigurována a je přístupný z prohlížeče na místním počítači v `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="ae22b-183">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="ae22b-184">Kontrola hlavičky odpovědi **Server** hlavička označuje, že aplikace ASP.NET Core je poskytovaný Kestrel:</span><span class="sxs-lookup"><span data-stu-id="ae22b-184">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="ae22b-185">Zobrazení protokolů</span><span class="sxs-lookup"><span data-stu-id="ae22b-185">Viewing logs</span></span>

<span data-ttu-id="ae22b-186">Od webové aplikace pomocí Kestrel se spravuje pomocí *systemd*, události a procesy jsou protokolovány centralizované deníku.</span><span class="sxs-lookup"><span data-stu-id="ae22b-186">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="ae22b-187">Ale tento deník obsahuje záznamy pro všechny služby a spravuje procesy *systemd*.</span><span class="sxs-lookup"><span data-stu-id="ae22b-187">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="ae22b-188">Chcete-li zobrazit `kestrel-hellomvc.service`-konkrétní položky, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ae22b-188">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="ae22b-189">Pro filtrování podle času, zadejte možnosti pomocí příkazu.</span><span class="sxs-lookup"><span data-stu-id="ae22b-189">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="ae22b-190">Například použít `--since today` filtrovat aktuální den nebo `--until 1 hour ago` zobrazíte položky do předchozí hodiny.</span><span class="sxs-lookup"><span data-stu-id="ae22b-190">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="ae22b-191">Další informace najdete v tématu [man stránka journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="ae22b-191">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="ae22b-192">Zabezpečení aplikace</span><span class="sxs-lookup"><span data-stu-id="ae22b-192">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="ae22b-193">Konfigurace brány firewall</span><span class="sxs-lookup"><span data-stu-id="ae22b-193">Configure firewall</span></span>

<span data-ttu-id="ae22b-194">*Firewalld* je dynamické démon ke správě brány firewall s podporou zón sítě.</span><span class="sxs-lookup"><span data-stu-id="ae22b-194">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="ae22b-195">Porty a filtrování paketů můžete dál spravovat iptables.</span><span class="sxs-lookup"><span data-stu-id="ae22b-195">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="ae22b-196">*Firewalld* by měl být ve výchozím nastavení nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="ae22b-196">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="ae22b-197">`yum` slouží k instalaci balíčku nebo ověřte, že je nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="ae22b-197">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="ae22b-198">Použití `firewalld` otevřít pouze porty potřebné pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ae22b-198">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="ae22b-199">V takovém případě je to port 80 a 443 jsou používány.</span><span class="sxs-lookup"><span data-stu-id="ae22b-199">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="ae22b-200">Následující příkazy trvale nastavte porty 80 a 443. Chcete-li otevřít:</span><span class="sxs-lookup"><span data-stu-id="ae22b-200">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="ae22b-201">Znovu načte nastavení brány firewall.</span><span class="sxs-lookup"><span data-stu-id="ae22b-201">Reload the firewall settings.</span></span> <span data-ttu-id="ae22b-202">Zkontrolujte dostupné služby a porty ve výchozí zóně.</span><span class="sxs-lookup"><span data-stu-id="ae22b-202">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="ae22b-203">Možnosti jsou k dispozici zkontrolováním `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="ae22b-203">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="ae22b-204">Konfigurace SSL</span><span class="sxs-lookup"><span data-stu-id="ae22b-204">SSL configuration</span></span>

<span data-ttu-id="ae22b-205">Chcete-li nakonfigurovat i Apache pro protokol SSL, *mod_ssl* modul se používá.</span><span class="sxs-lookup"><span data-stu-id="ae22b-205">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="ae22b-206">Když *httpd* modul byl nainstalován, *mod_ssl* také nainstalován modul.</span><span class="sxs-lookup"><span data-stu-id="ae22b-206">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="ae22b-207">Pokud nebyla nainstalována, použijte `yum` přidejte do konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ae22b-207">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="ae22b-208">K vynucení SSL, nainstalujte `mod_rewrite` modulu, který chcete-li povolit přepisování adres URL:</span><span class="sxs-lookup"><span data-stu-id="ae22b-208">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="ae22b-209">Upravit *hellomvc.conf* soubor povolit přepisování adres URL a zabezpečenou komunikaci na portu 443:</span><span class="sxs-lookup"><span data-stu-id="ae22b-209">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
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
> <span data-ttu-id="ae22b-210">Tento příklad používá místně vygeneruje certifikát.</span><span class="sxs-lookup"><span data-stu-id="ae22b-210">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="ae22b-211">**SSLCertificateFile** by měl být soubor primárního certifikátu pro název domény.</span><span class="sxs-lookup"><span data-stu-id="ae22b-211">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="ae22b-212">**SSLCertificateKeyFile** by měl být soubor s klíčem vygenerován při vytvoření žádosti o podepsání certifikátu.</span><span class="sxs-lookup"><span data-stu-id="ae22b-212">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="ae22b-213">**SSLCertificateChainFile** by měl být soubor zprostředkující certifikát (pokud existuje), který byl zadán certifikační autoritou.</span><span class="sxs-lookup"><span data-stu-id="ae22b-213">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="ae22b-214">Uložte soubor a otestujte konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="ae22b-214">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="ae22b-215">Restartujte Apache:</span><span class="sxs-lookup"><span data-stu-id="ae22b-215">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="ae22b-216">Další návrhy Apache</span><span class="sxs-lookup"><span data-stu-id="ae22b-216">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="ae22b-217">Dodatečné hlavičky</span><span class="sxs-lookup"><span data-stu-id="ae22b-217">Additional headers</span></span>

<span data-ttu-id="ae22b-218">Myslet při zabezpečování před škodlivými útoky, existuje pár hlavičky, které se musí buď být přidá nebo upraví.</span><span class="sxs-lookup"><span data-stu-id="ae22b-218">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="ae22b-219">Ujistěte se, `mod_headers` je nainstalován modul:</span><span class="sxs-lookup"><span data-stu-id="ae22b-219">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="ae22b-220">Zabezpečení před útoky útoků typu clickjacking Apache</span><span class="sxs-lookup"><span data-stu-id="ae22b-220">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="ae22b-221">[Útoků typu Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), označované také jako *uživatelského rozhraní zjednávání nápravy útoku*, je napadením se zlými úmysly, kde návštěvníků webu je nalákaní, odkaz nebo tlačítko na stránce jiné než aktuálně navštívený.</span><span class="sxs-lookup"><span data-stu-id="ae22b-221">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="ae22b-222">Použití `X-FRAME-OPTIONS` k zabezpečení webu.</span><span class="sxs-lookup"><span data-stu-id="ae22b-222">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="ae22b-223">Upravit *httpd.conf* souboru:</span><span class="sxs-lookup"><span data-stu-id="ae22b-223">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="ae22b-224">Přidejte řádek `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="ae22b-224">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="ae22b-225">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="ae22b-225">Save the file.</span></span> <span data-ttu-id="ae22b-226">Restartujte Apache.</span><span class="sxs-lookup"><span data-stu-id="ae22b-226">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="ae22b-227">Typ MIME pro analýzu sítě</span><span class="sxs-lookup"><span data-stu-id="ae22b-227">MIME-type sniffing</span></span>

<span data-ttu-id="ae22b-228">`X-Content-Type-Options` Záhlaví brání aplikaci Internet Explorer z *MIME pro analýzu sítě* (určení souboru `Content-Type` z obsahu souboru).</span><span class="sxs-lookup"><span data-stu-id="ae22b-228">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="ae22b-229">Pokud server nastaví `Content-Type` záhlaví `text/html` s `nosniff` sadu možností, Internet Explorer vykreslí obsah jako `text/html` bez ohledu na jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="ae22b-229">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="ae22b-230">Upravit *httpd.conf* souboru:</span><span class="sxs-lookup"><span data-stu-id="ae22b-230">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="ae22b-231">Přidejte řádek `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="ae22b-231">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="ae22b-232">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="ae22b-232">Save the file.</span></span> <span data-ttu-id="ae22b-233">Restartujte Apache.</span><span class="sxs-lookup"><span data-stu-id="ae22b-233">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="ae22b-234">Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="ae22b-234">Load Balancing</span></span>

<span data-ttu-id="ae22b-235">Tento příklad ukazuje, jak nainstalovat a nakonfigurovat Apache na CentOS 7 a Kestrel ve stejném počítači instance.</span><span class="sxs-lookup"><span data-stu-id="ae22b-235">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="ae22b-236">Aby bylo možné, není nutné jediný bod selhání; pomocí *mod_proxy_balancer* a úpravy **VirtualHost** by umožňoval správu více instancí služby web apps za proxy serverem Apache.</span><span class="sxs-lookup"><span data-stu-id="ae22b-236">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="ae22b-237">V konfiguračním souboru je znázorněno níže, další instanci `hellomvc` aplikace je ke spuštění na portu 5001 instalace.</span><span class="sxs-lookup"><span data-stu-id="ae22b-237">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="ae22b-238">*Proxy* části je nastaven s konfigurací nástroje pro vyrovnávání se dvěma členy pro vyrovnávání zatížení *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="ae22b-238">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
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

### <a name="rate-limits"></a><span data-ttu-id="ae22b-239">Omezení přenosové rychlosti</span><span class="sxs-lookup"><span data-stu-id="ae22b-239">Rate Limits</span></span>

<span data-ttu-id="ae22b-240">Pomocí *mod_ratelimit*, které je součástí *httpd* modulu, šířky pásma klientů může být omezená:</span><span class="sxs-lookup"><span data-stu-id="ae22b-240">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="ae22b-241">Příklad souboru omezuje šířku pásma jako 600 KB/s v části kořenový adresář:</span><span class="sxs-lookup"><span data-stu-id="ae22b-241">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

## <a name="additional-resources"></a><span data-ttu-id="ae22b-242">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ae22b-242">Additional resources</span></span>

* [<span data-ttu-id="ae22b-243">Konfigurace ASP.NET Core práci se servery proxy a nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="ae22b-243">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
