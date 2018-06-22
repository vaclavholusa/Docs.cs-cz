---
title: Hostování v systému Linux s Apache ASP.NET Core
description: Zjistěte, jak nastavit Apache jako reverzní proxy server na CentOS a přesměrování provozu HTTP na webové aplikace ASP.NET Core systémem Kestrel.
author: spboyer
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 69e92af08eabede023608e612f1fbd48a8f2608e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275448"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="81e5e-103">Hostování v systému Linux s Apache ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81e5e-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="81e5e-104">Podle [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="81e5e-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="81e5e-105">Pomocí tohoto průvodce, zjistěte, jak nastavit [Apache](https://httpd.apache.org/) jako reverzní proxy server na [CentOS 7](https://www.centos.org/) a přesměrování provozu HTTP na webové aplikace ASP.NET Core systémem [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="81e5e-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="81e5e-106">[Mod_proxy rozšíření](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) a související moduly vytvořit reverzního proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="81e5e-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81e5e-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="81e5e-107">Prerequisites</span></span>

1. <span data-ttu-id="81e5e-108">Server se službou CentOS 7 s standardní uživatelský účet s oprávněním sudo.</span><span class="sxs-lookup"><span data-stu-id="81e5e-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="81e5e-109">Nainstalujte na .NET Core runtime na serveru.</span><span class="sxs-lookup"><span data-stu-id="81e5e-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="81e5e-110">Přejděte [.NET Core všechny soubory ke stažení stránky](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="81e5e-110">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="81e5e-111">Vyberte ze seznamu v části nejnovější modul runtime bez preview **Runtime**.</span><span class="sxs-lookup"><span data-stu-id="81e5e-111">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="81e5e-112">Vyberte a postupujte podle pokynů pro CentOS nebo Oracle.</span><span class="sxs-lookup"><span data-stu-id="81e5e-112">Select and follow the instructions for CentOS/Oracle.</span></span>
1. <span data-ttu-id="81e5e-113">Stávající aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="81e5e-113">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="81e5e-114">Publikování a zkopírujte přes aplikace</span><span class="sxs-lookup"><span data-stu-id="81e5e-114">Publish and copy over the app</span></span>

<span data-ttu-id="81e5e-115">Konfigurace aplikace pro [nasazení závislé na framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="81e5e-115">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="81e5e-116">Spustit [dotnet publikování](/dotnet/core/tools/dotnet-publish) z vývojového prostředí pro zabalení aplikace do adresáře (například *Koš a verze nebo&lt;target_framework_moniker&gt;/ publikovat*), můžete Spusťte na serveru:</span><span class="sxs-lookup"><span data-stu-id="81e5e-116">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="81e5e-117">Aplikace lze také publikovat jako [samostatná nasazení](/dotnet/core/deploying/#self-contained-deployments-scd) Pokud nechcete udržovat na .NET Core runtime na serveru.</span><span class="sxs-lookup"><span data-stu-id="81e5e-117">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="81e5e-118">Zkopírujte aplikace ASP.NET Core k serveru pomocí nástroje, který se integruje do pracovního postupu organizace (například spojovací bod služby, pomocí protokolu SFTP).</span><span class="sxs-lookup"><span data-stu-id="81e5e-118">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="81e5e-119">Je běžné najít webové aplikace v rámci *var* directory (například *aspnetcore/var/hellomvc*).</span><span class="sxs-lookup"><span data-stu-id="81e5e-119">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="81e5e-120">V části nasazení produkční scénář průběžnou integraci pracovní postup funguje publikování aplikace a kopírování prostředků na server.</span><span class="sxs-lookup"><span data-stu-id="81e5e-120">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="81e5e-121">Konfigurace proxy serveru</span><span class="sxs-lookup"><span data-stu-id="81e5e-121">Configure a proxy server</span></span>

<span data-ttu-id="81e5e-122">Reverzní proxy server je běžné instalační program pro obsluhující dynamické webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="81e5e-122">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="81e5e-123">Reverzní proxy server ukončí požadavek HTTP a předává do aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="81e5e-123">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="81e5e-124">Proxy server je takovou, která předá požadavky klientů na jiný server místo splnění požadavků sám sebe.</span><span class="sxs-lookup"><span data-stu-id="81e5e-124">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="81e5e-125">Reverzní proxy server předává dlouhodobý cíl, obvykle jménem libovolného klientů.</span><span class="sxs-lookup"><span data-stu-id="81e5e-125">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="81e5e-126">V této příručce je Apache nakonfigurovaný jako reverzní proxy server spuštěn na stejném serveru, aby Kestrel obsluhuje aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="81e5e-126">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="81e5e-127">Protože požadavky jsou předávány podle reverzní proxy server, použijte [předané Middleware hlavičky](xref:host-and-deploy/proxy-load-balancer) z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="81e5e-127">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="81e5e-128">Middleware aktualizace `Request.Scheme`pomocí `X-Forwarded-Proto` záhlaví, tak, že přesměrování identifikátory URI a jiné zásady zabezpečení pracovat správně.</span><span class="sxs-lookup"><span data-stu-id="81e5e-128">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="81e5e-129">Všechny součásti, které závisí na schéma, jako je například ověřování, generování odkazů, přesměrování a informace o zeměpisné poloze, musí být umístěny po vyvolání hlavičky Middleware předávat.</span><span class="sxs-lookup"><span data-stu-id="81e5e-129">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="81e5e-130">Obecně platí by měla předávat Middleware hlavičky spustit před další middleware s výjimkou diagnostiky a zpracování middleware chyb.</span><span class="sxs-lookup"><span data-stu-id="81e5e-130">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="81e5e-131">Toto uspořádání zajistí, že middleware spoléhat na informace předávané hlavičky spotřebovat hodnoty hlavičky pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="81e5e-131">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

::: moniker range=">= aspnetcore-2.0"
> [!NOTE]
> <span data-ttu-id="81e5e-132">Buď konfiguraci&mdash;s nebo bez reverzní proxy server&mdash;je platný a podporované konfigurace hostování pro technologii ASP.NET Core 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="81e5e-132">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="81e5e-133">Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="81e5e-133">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>
::: moniker-end

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81e5e-134">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="81e5e-134">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="81e5e-135">Vyvolání [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metoda v `Startup.Configure` před voláním [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) nebo podobné middleware ověřování schématu.</span><span class="sxs-lookup"><span data-stu-id="81e5e-135">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="81e5e-136">Nakonfigurujte middleware předávat `X-Forwarded-For` a `X-Forwarded-Proto` hlavičky:</span><span class="sxs-lookup"><span data-stu-id="81e5e-136">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81e5e-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81e5e-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="81e5e-138">Vyvolání [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metoda v `Startup.Configure` před voláním [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) a [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) nebo podobné schéma ověřování middleware.</span><span class="sxs-lookup"><span data-stu-id="81e5e-138">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="81e5e-139">Nakonfigurujte middleware předávat `X-Forwarded-For` a `X-Forwarded-Proto` hlavičky:</span><span class="sxs-lookup"><span data-stu-id="81e5e-139">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="81e5e-140">Pokud žádné [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) jsou určené pro middleware, jsou výchozí hlavičky předávat `None`.</span><span class="sxs-lookup"><span data-stu-id="81e5e-140">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="81e5e-141">Další konfigurace může být potřeba pro aplikace, které jsou hostovány za proxy servery a nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="81e5e-141">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="81e5e-142">Další informace najdete v tématu [konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="81e5e-142">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="81e5e-143">Nainstalujte Apache</span><span class="sxs-lookup"><span data-stu-id="81e5e-143">Install Apache</span></span>

<span data-ttu-id="81e5e-144">CentOS balíčky aktualizací pro jejich nejnovější stabilní verze:</span><span class="sxs-lookup"><span data-stu-id="81e5e-144">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="81e5e-145">Instalace webového serveru Apache na CentOS s jedním `yum` příkaz:</span><span class="sxs-lookup"><span data-stu-id="81e5e-145">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="81e5e-146">Ukázka výstupu po spuštění příkazu:</span><span class="sxs-lookup"><span data-stu-id="81e5e-146">Sample output after running the command:</span></span>

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
> <span data-ttu-id="81e5e-147">V tomto příkladu výstupu odráží httpd.86_64 vzhledem k tomu, že verze CentOS 7 je 64bitový.</span><span class="sxs-lookup"><span data-stu-id="81e5e-147">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="81e5e-148">Pokud chcete ověřit, kde je nainstalován Apache, spusťte `whereis httpd` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="81e5e-148">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="81e5e-149">Konfigurace Apache</span><span class="sxs-lookup"><span data-stu-id="81e5e-149">Configure Apache</span></span>

<span data-ttu-id="81e5e-150">Konfigurační soubory pro Apache se nacházejí v rámci `/etc/httpd/conf.d/` adresáře.</span><span class="sxs-lookup"><span data-stu-id="81e5e-150">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="81e5e-151">Všechny soubory s *.conf* rozšíření jsou zpracovávány v abecedním pořadí kromě souborů konfigurace modulu v `/etc/httpd/conf.modules.d/`, která obsahuje všechny konfigurační soubory nezbytné k načtení moduly.</span><span class="sxs-lookup"><span data-stu-id="81e5e-151">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="81e5e-152">Vytvoření konfiguračního souboru s názvem *hellomvc.conf*, pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="81e5e-152">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

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

<span data-ttu-id="81e5e-153">`VirtualHost` Bloku může vyskytovat více než jednou. v jedné nebo více souborů na serveru.</span><span class="sxs-lookup"><span data-stu-id="81e5e-153">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="81e5e-154">V předchozím konfiguračního souboru přijímá Apache veřejné přenosy na portu 80.</span><span class="sxs-lookup"><span data-stu-id="81e5e-154">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="81e5e-155">Domény `www.example.com` je zpracování a `*.example.com` alias přeloží na stejné webové stránky.</span><span class="sxs-lookup"><span data-stu-id="81e5e-155">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="81e5e-156">V tématu [podporu na základě názvu virtuálního hostitele](https://httpd.apache.org/docs/current/vhosts/name-based.html) Další informace.</span><span class="sxs-lookup"><span data-stu-id="81e5e-156">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="81e5e-157">Požadavky jsou směrovány přes proxy server v kořenovém adresáři na port 5000 serveru na 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="81e5e-157">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="81e5e-158">Pro obousměrnou komunikaci `ProxyPass` a `ProxyPassReverse` jsou povinné.</span><span class="sxs-lookup"><span data-stu-id="81e5e-158">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span>

> [!WARNING]
> <span data-ttu-id="81e5e-159">Nepodařilo se určit správný [ServerName direktiva](https://httpd.apache.org/docs/current/mod/core.html#servername) v **VirtualHost** bloku zpřístupní v aplikaci ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="81e5e-159">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="81e5e-160">Vazba subdomény zástupný znak (například `*.example.com`) nemá představovat toto bezpečnostní riziko, pokud řízení celého nadřazené domény (Naproti tomu `*.com`, což je snadno napadnutelný).</span><span class="sxs-lookup"><span data-stu-id="81e5e-160">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="81e5e-161">V tématu [rfc7230 části-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Další informace.</span><span class="sxs-lookup"><span data-stu-id="81e5e-161">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="81e5e-162">Můžete konfigurovat protokolování pro jednotlivé `VirtualHost` pomocí `ErrorLog` a `CustomLog` direktivy.</span><span class="sxs-lookup"><span data-stu-id="81e5e-162">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="81e5e-163">`ErrorLog` je umístění, kam server protokoluje chyby, a `CustomLog` nastaví název souboru a formát souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="81e5e-163">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="81e5e-164">V takovém případě je kde požadavku je do něj protokolují informace.</span><span class="sxs-lookup"><span data-stu-id="81e5e-164">In this case, this is where request information is logged.</span></span> <span data-ttu-id="81e5e-165">Je jeden řádek pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="81e5e-165">There's one line for each request.</span></span>

<span data-ttu-id="81e5e-166">Uložte tento soubor a otestovat konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="81e5e-166">Save the file and test the configuration.</span></span> <span data-ttu-id="81e5e-167">Pokud vše projde, odpověď by měla být `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="81e5e-167">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="81e5e-168">Restartujte Apache:</span><span class="sxs-lookup"><span data-stu-id="81e5e-168">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="81e5e-169">Monitorování aplikace</span><span class="sxs-lookup"><span data-stu-id="81e5e-169">Monitoring the app</span></span>

<span data-ttu-id="81e5e-170">Apache je teď instalace předávat požadavky na `http://localhost:80` systémem Kestrel v aplikaci ASP.NET Core `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="81e5e-170">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="81e5e-171">Apache není však nastavit ke správě procesu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="81e5e-171">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="81e5e-172">Použití *systemd* a vytvořte soubor služby spuštění a sledování základní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="81e5e-172">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="81e5e-173">*systemd* je init systém, který poskytuje mnoho výkonné funkce pro spouštění, zastavování a Správa procesů.</span><span class="sxs-lookup"><span data-stu-id="81e5e-173">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="81e5e-174">Vytvoření souboru služby</span><span class="sxs-lookup"><span data-stu-id="81e5e-174">Create the service file</span></span>

<span data-ttu-id="81e5e-175">Vytvořte soubor definice služby:</span><span class="sxs-lookup"><span data-stu-id="81e5e-175">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="81e5e-176">Soubor službu příkladu pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="81e5e-176">An example service file for the app:</span></span>

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
> <span data-ttu-id="81e5e-177">**Uživatel** &mdash; Pokud uživatel *apache* nepoužívá konfigurace, musí být uživatel nejprve a pro soubory zadané správné vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="81e5e-177">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

> [!NOTE]
> <span data-ttu-id="81e5e-178">Některé hodnoty (například připojovací řetězce SQL), je nutné uvést pro zprostředkovatele konfigurace pro čtení proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="81e5e-178">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="81e5e-179">Použijte následující příkaz k vygenerování správně uvozený hodnoty pro použití v konfiguračním souboru:</span><span class="sxs-lookup"><span data-stu-id="81e5e-179">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="81e5e-180">Uložte tento soubor a povolení služby:</span><span class="sxs-lookup"><span data-stu-id="81e5e-180">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="81e5e-181">Spusťte službu a ověřte, zda je spuštěna:</span><span class="sxs-lookup"><span data-stu-id="81e5e-181">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="81e5e-182">S reverzní proxy server nakonfigurovat a spravovat prostřednictvím Kestrel *systemd*, webové aplikace je plně nakonfigurovaná a je přístupný z prohlížeče v místním počítači v `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="81e5e-182">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="81e5e-183">Probíhá kontrola hlavičky odpovědi **Server** Hlavička uvádí, že je aplikace ASP.NET Core poskytovaný Kestrel:</span><span class="sxs-lookup"><span data-stu-id="81e5e-183">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="81e5e-184">Prohlížení protokolů</span><span class="sxs-lookup"><span data-stu-id="81e5e-184">Viewing logs</span></span>

<span data-ttu-id="81e5e-185">Od webové aplikace pomocí Kestrel je spravovat pomocí *systemd*, procesy a událostí jsou zaznamenány do centralizované deníku.</span><span class="sxs-lookup"><span data-stu-id="81e5e-185">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="81e5e-186">Ale tento deník obsahuje položky pro všechny služby a spravuje procesy *systemd*.</span><span class="sxs-lookup"><span data-stu-id="81e5e-186">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="81e5e-187">Chcete-li zobrazit `kestrel-hellomvc.service`-konkrétní položky, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="81e5e-187">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="81e5e-188">Pro filtrování podle času, zadejte čas možnosti pomocí příkazu.</span><span class="sxs-lookup"><span data-stu-id="81e5e-188">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="81e5e-189">Například použít `--since today` filtrovat aktuální den nebo `--until 1 hour ago` zobrazíte položky do předchozí hodiny.</span><span class="sxs-lookup"><span data-stu-id="81e5e-189">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="81e5e-190">Další informace najdete v tématu [man stránky pro journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="81e5e-190">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="81e5e-191">Zabezpečení aplikací</span><span class="sxs-lookup"><span data-stu-id="81e5e-191">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="81e5e-192">Konfigurace brány firewall</span><span class="sxs-lookup"><span data-stu-id="81e5e-192">Configure firewall</span></span>

<span data-ttu-id="81e5e-193">*Firewalld* je dynamická démon ke správě brány firewall s podporou zón sítě.</span><span class="sxs-lookup"><span data-stu-id="81e5e-193">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="81e5e-194">Porty a filtrování paketů můžete dál spravovat přes iptables.</span><span class="sxs-lookup"><span data-stu-id="81e5e-194">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="81e5e-195">*Firewalld* by měly být nainstalovány ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="81e5e-195">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="81e5e-196">`yum` slouží k instalaci balíčku nebo ověřte, zda že je nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="81e5e-196">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="81e5e-197">Použití `firewalld` k otevření pouze portů, které jsou potřebné pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="81e5e-197">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="81e5e-198">V takovém případě je to port 80 a 443 jsou používány.</span><span class="sxs-lookup"><span data-stu-id="81e5e-198">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="81e5e-199">Následující příkazy trvale nastavte porty 80 a 443 otevřete:</span><span class="sxs-lookup"><span data-stu-id="81e5e-199">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="81e5e-200">Znovu načte nastavení brány firewall.</span><span class="sxs-lookup"><span data-stu-id="81e5e-200">Reload the firewall settings.</span></span> <span data-ttu-id="81e5e-201">Zkontrolujte dostupné služby a porty ve výchozí zóně.</span><span class="sxs-lookup"><span data-stu-id="81e5e-201">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="81e5e-202">Možnosti jsou k dispozici zkontrolováním `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="81e5e-202">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="81e5e-203">Konfigurace protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="81e5e-203">SSL configuration</span></span>

<span data-ttu-id="81e5e-204">Ke konfiguraci Apache pro protokol SSL, *mod_ssl* modul se používá.</span><span class="sxs-lookup"><span data-stu-id="81e5e-204">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="81e5e-205">Když *httpd* byl nainstalován modul, *mod_ssl* také nainstalován modul.</span><span class="sxs-lookup"><span data-stu-id="81e5e-205">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="81e5e-206">Pokud nebyla nainstalovaná, použijte `yum` tím ho přidáte do konfigurace.</span><span class="sxs-lookup"><span data-stu-id="81e5e-206">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="81e5e-207">Pokud chcete vynutit SSL, nainstalujte `mod_rewrite` modulu, chcete-li povolit přepisování adres URL:</span><span class="sxs-lookup"><span data-stu-id="81e5e-207">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="81e5e-208">Změnit *hellomvc.conf* souboru povolit přepisování adres URL a zabezpečenou komunikaci na portu 443:</span><span class="sxs-lookup"><span data-stu-id="81e5e-208">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="81e5e-209">Tento příklad používá certifikát, místně generované.</span><span class="sxs-lookup"><span data-stu-id="81e5e-209">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="81e5e-210">**SSLCertificateFile** by měl být soubor primární certifikát pro název domény.</span><span class="sxs-lookup"><span data-stu-id="81e5e-210">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="81e5e-211">**SSLCertificateKeyFile** by měl být soubor klíče vygenerován při vytváření zástupce oddělení služeb zákazníkům.</span><span class="sxs-lookup"><span data-stu-id="81e5e-211">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="81e5e-212">**SSLCertificateChainFile** by měla být soubor zprostředkující certifikátu (pokud existuje), byl zadán certifikační autoritou.</span><span class="sxs-lookup"><span data-stu-id="81e5e-212">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="81e5e-213">Uložte tento soubor a otestovat konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="81e5e-213">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="81e5e-214">Restartujte Apache:</span><span class="sxs-lookup"><span data-stu-id="81e5e-214">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="81e5e-215">Další návrhy Apache</span><span class="sxs-lookup"><span data-stu-id="81e5e-215">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="81e5e-216">Další záhlaví</span><span class="sxs-lookup"><span data-stu-id="81e5e-216">Additional headers</span></span>

<span data-ttu-id="81e5e-217">Chcete-li zabezpečení před útoky se zlými úmysly, jsou několik hlavičky, které by měl být změnit nebo přidat.</span><span class="sxs-lookup"><span data-stu-id="81e5e-217">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="81e5e-218">Ujistěte se, že `mod_headers` je modul nainstalován:</span><span class="sxs-lookup"><span data-stu-id="81e5e-218">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="81e5e-219">Apache zabezpečení před útoky útoků typu clickjacking</span><span class="sxs-lookup"><span data-stu-id="81e5e-219">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="81e5e-220">[Útoků typu Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), také známé jako *uživatelského rozhraní zjednávání nápravy útoku*, je napadením se zlými úmysly, kde je oklame kliknutím na odkaz nebo tlačítko na stránce jiné než aktuálně navštívený, aby návštěvník webu.</span><span class="sxs-lookup"><span data-stu-id="81e5e-220">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="81e5e-221">Použití `X-FRAME-OPTIONS` k zabezpečení webu.</span><span class="sxs-lookup"><span data-stu-id="81e5e-221">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="81e5e-222">Upravit *httpd.conf* souboru:</span><span class="sxs-lookup"><span data-stu-id="81e5e-222">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="81e5e-223">Přidejte řádek `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="81e5e-223">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="81e5e-224">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="81e5e-224">Save the file.</span></span> <span data-ttu-id="81e5e-225">Restartujte Apache.</span><span class="sxs-lookup"><span data-stu-id="81e5e-225">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="81e5e-226">Sledování toku dat typ MIME</span><span class="sxs-lookup"><span data-stu-id="81e5e-226">MIME-type sniffing</span></span>

<span data-ttu-id="81e5e-227">`X-Content-Type-Options` Záhlaví brání aplikaci Internet Explorer z *sledování toku dat MIME* (určení souboru `Content-Type` z obsahu souboru).</span><span class="sxs-lookup"><span data-stu-id="81e5e-227">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="81e5e-228">Pokud je server nastaví `Content-Type` hlavičky k `text/html` s `nosniff` sadu možností, Internet Explorer vykreslí obsah jako `text/html` bez ohledu na jeho obsahu.</span><span class="sxs-lookup"><span data-stu-id="81e5e-228">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="81e5e-229">Upravit *httpd.conf* souboru:</span><span class="sxs-lookup"><span data-stu-id="81e5e-229">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="81e5e-230">Přidejte řádek `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="81e5e-230">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="81e5e-231">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="81e5e-231">Save the file.</span></span> <span data-ttu-id="81e5e-232">Restartujte Apache.</span><span class="sxs-lookup"><span data-stu-id="81e5e-232">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="81e5e-233">Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="81e5e-233">Load Balancing</span></span>

<span data-ttu-id="81e5e-234">Tento příklad ukazuje, jak nainstalovat a nakonfigurovat Apache na CentOS 7 a Kestrel na stejném počítači instance.</span><span class="sxs-lookup"><span data-stu-id="81e5e-234">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="81e5e-235">Chcete-li nemá jediný bod selhání; pomocí *mod_proxy_balancer* a úprava **VirtualHost** by umožňoval správu více instancí webové aplikace za proxy serverem Apache.</span><span class="sxs-lookup"><span data-stu-id="81e5e-235">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="81e5e-236">V konfiguračním souboru vidíte níže, další instanci `hellomvc` aplikace je ke spuštění na portu 5001 instalace.</span><span class="sxs-lookup"><span data-stu-id="81e5e-236">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="81e5e-237">*Proxy* části nastavena konfigurace služby Vyrovnávání se dva členy pro vyrovnávání zatížení *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="81e5e-237">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="81e5e-238">Omezení přenosové rychlosti</span><span class="sxs-lookup"><span data-stu-id="81e5e-238">Rate Limits</span></span>

<span data-ttu-id="81e5e-239">Pomocí *mod_ratelimit*, který je součástí *httpd* modulu, šířku pásma klientů může být omezen:</span><span class="sxs-lookup"><span data-stu-id="81e5e-239">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="81e5e-240">Příklad souboru omezení šířky pásma jako 600 KB/s v části umístění kořenového adresáře:</span><span class="sxs-lookup"><span data-stu-id="81e5e-240">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

## <a name="additional-resources"></a><span data-ttu-id="81e5e-241">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="81e5e-241">Additional resources</span></span>

* [<span data-ttu-id="81e5e-242">Konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="81e5e-242">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
