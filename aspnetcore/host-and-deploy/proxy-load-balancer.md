---
title: Konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení
author: guardrex
description: Další informace o konfiguraci pro aplikace, které jsou hostovány za proxy servery a požadovat služby Vyrovnávání zatížení, které často skrývat důležité informace.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: b153a7406ae1b31a2aa453135c6bd0e5ce0b2997
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/30/2018
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a><span data-ttu-id="e6239-103">Konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="e6239-103">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>

<span data-ttu-id="e6239-104">Podle [Luke Latham](https://github.com/guardrex) a [Ross Jan](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="e6239-104">By [Luke Latham](https://github.com/guardrex) and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="e6239-105">Doporučenou konfiguraci pro jádro ASP.NET je aplikace hostované pomocí modulu jádra IIS/ASP.NET, Nginx nebo Apache.</span><span class="sxs-lookup"><span data-stu-id="e6239-105">In the recommended configuration for ASP.NET Core, the app is hosted using IIS/ASP.NET Core Module, Nginx, or Apache.</span></span> <span data-ttu-id="e6239-106">Proxy servery, nástroje pro vyrovnávání zatížení a jiných síťových zařízení často skrývat informace o požadavku předtím, než dorazí do aplikace:</span><span class="sxs-lookup"><span data-stu-id="e6239-106">Proxy servers, load balancers, and other network appliances often obscure information about the request before it reaches the app:</span></span>

* <span data-ttu-id="e6239-107">Pokud požadavky HTTPS jsou směrovány přes proxy server pomocí protokolu HTTP, původní schématu (HTTPS) dojde ke ztrátě a musí být přeposílán v hlavičce.</span><span class="sxs-lookup"><span data-stu-id="e6239-107">When HTTPS requests are proxied over HTTP, the original scheme (HTTPS) is lost and must be forwarded in a header.</span></span>
* <span data-ttu-id="e6239-108">Protože aplikace obdrží žádost z proxy serveru a na Internetu nebo podnikové síti není true zdroj, musí v hlavičce předají zdrojovou IP adresu klienta.</span><span class="sxs-lookup"><span data-stu-id="e6239-108">Because an app receives a request from the proxy and not its true source on the Internet or corporate network, the originating client IP address must also be forwarded in a header.</span></span>

<span data-ttu-id="e6239-109">Tyto informace může být důležité v zpracování žádostí, například v přesměrování, ověřování, generování odkazů, vyhodnocení zásad a geoloation klienta.</span><span class="sxs-lookup"><span data-stu-id="e6239-109">This information may be important in request processing, for example in redirects, authentication, link generation, policy evaluation, and client geoloation.</span></span>

## <a name="forwarded-headers"></a><span data-ttu-id="e6239-110">Přesměrovaná hlavičky</span><span class="sxs-lookup"><span data-stu-id="e6239-110">Forwarded headers</span></span>

<span data-ttu-id="e6239-111">Podle konvence proxy předávat informace v hlavičkách protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="e6239-111">By convention, proxies forward information in HTTP headers.</span></span>

| <span data-ttu-id="e6239-112">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="e6239-112">Header</span></span> | <span data-ttu-id="e6239-113">Popis</span><span class="sxs-lookup"><span data-stu-id="e6239-113">Description</span></span> |
| ------ | ----------- |
| <span data-ttu-id="e6239-114">X-předávaných pro</span><span class="sxs-lookup"><span data-stu-id="e6239-114">X-Forwarded-For</span></span> | <span data-ttu-id="e6239-115">Obsahuje informace o klientovi, který spustil požadavků a následné proxy v řetězu serverů proxy.</span><span class="sxs-lookup"><span data-stu-id="e6239-115">Holds information about the client that initiated the request and subsequent proxies in a chain of proxies.</span></span> <span data-ttu-id="e6239-116">Tento parametr může obsahovat IP adresy (a volitelně čísla portů).</span><span class="sxs-lookup"><span data-stu-id="e6239-116">This parameter may contain IP addresses (and, optionally, port numbers).</span></span> <span data-ttu-id="e6239-117">V řetězu proxy servery určuje první parametr klienta, kde byl nejprve požadavek.</span><span class="sxs-lookup"><span data-stu-id="e6239-117">In a chain of proxy servers, the first parameter indicates the client where the request was first made.</span></span> <span data-ttu-id="e6239-118">Postupujte podle následujících proxy identifikátory.</span><span class="sxs-lookup"><span data-stu-id="e6239-118">Subsequent proxy identifiers follow.</span></span> <span data-ttu-id="e6239-119">Poslední proxy server v řetězu není v seznamu parametrů.</span><span class="sxs-lookup"><span data-stu-id="e6239-119">The last proxy in the chain isn't in the list of parameters.</span></span> <span data-ttu-id="e6239-120">Poslední proxy IP adresu a volitelně číslo portu, jsou k dispozici jako vzdálené IP adresy v přenosové vrstvě.</span><span class="sxs-lookup"><span data-stu-id="e6239-120">The last proxy's IP address, and optionally a port number, are available as the remote IP address at the transport layer.</span></span> |
| <span data-ttu-id="e6239-121">X-Forwarded-Proto</span><span class="sxs-lookup"><span data-stu-id="e6239-121">X-Forwarded-Proto</span></span> | <span data-ttu-id="e6239-122">Hodnota původní schématu (HTTP či HTTPS).</span><span class="sxs-lookup"><span data-stu-id="e6239-122">The value of the originating scheme (HTTP/HTTPS).</span></span> <span data-ttu-id="e6239-123">Hodnota může být také seznam schémat, pokud požadavek projde více proxy serverů.</span><span class="sxs-lookup"><span data-stu-id="e6239-123">The value may also be a list of schemes if the request has traversed multiple proxies.</span></span> |
| <span data-ttu-id="e6239-124">X-Forwarded-Host</span><span class="sxs-lookup"><span data-stu-id="e6239-124">X-Forwarded-Host</span></span> | <span data-ttu-id="e6239-125">Původní hodnotu pole hlavičky hostitele.</span><span class="sxs-lookup"><span data-stu-id="e6239-125">The original value of the Host header field.</span></span> <span data-ttu-id="e6239-126">Obvykle proxy Neupravovat hlavičku hostitele.</span><span class="sxs-lookup"><span data-stu-id="e6239-126">Usually, proxies don't modify the Host header.</span></span> <span data-ttu-id="e6239-127">V tématu [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) informace o zvýšení oprávnění ohrožení zabezpečení, která má vliv na systémy, kde není ověření proxy serveru nebo hlavičky hostitele restict známé dobré hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e6239-127">See [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) for information on an elevation-of-privileges vulnerability that affects systems where the proxy doesn't validate or restict Host headers to known good values.</span></span> |

<span data-ttu-id="e6239-128">Middleware hlavičky předávány z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) balíčků, čte tyto hlavičky a vyplní přidružené pole na [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="e6239-128">The Forwarded Headers Middleware, from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package, reads these headers and fills in the associated fields on [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span> 

<span data-ttu-id="e6239-129">Middleware aktualizace:</span><span class="sxs-lookup"><span data-stu-id="e6239-129">The middleware updates:</span></span>

* <span data-ttu-id="e6239-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; nastavit pomocí `X-Forwarded-For` hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="e6239-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; Set using the `X-Forwarded-For` header value.</span></span> <span data-ttu-id="e6239-131">Další nastavení mají vliv jak middleware nastaví `RemoteIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="e6239-131">Additional settings influence how the middleware sets `RemoteIpAddress`.</span></span> <span data-ttu-id="e6239-132">Podrobnosti najdete v tématu [možnosti předávat middlewaru hlavičky](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="e6239-132">For details, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>
* <span data-ttu-id="e6239-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; nastavit pomocí `X-Forwarded-Proto` hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="e6239-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; Set using the `X-Forwarded-Proto` header value.</span></span>
* <span data-ttu-id="e6239-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; nastavit pomocí `X-Forwarded-Host` hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="e6239-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; Set using the `X-Forwarded-Host` header value.</span></span>

<span data-ttu-id="e6239-135">Všimněte si, že ne všechna síťová zařízení přidat `X-Forwarded-For` a `X-Forwarded-Proto` hlavičky bez další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e6239-135">Note that not all network appliances add the `X-Forwarded-For` and `X-Forwarded-Proto` headers without additional configuration.</span></span> <span data-ttu-id="e6239-136">Vyhledejte pokyny výrobce zařízení, pokud směrovány přes proxy server požadavky nesmí obsahovat tyto hlavičky po uplynutí aplikace.</span><span class="sxs-lookup"><span data-stu-id="e6239-136">Consult your appliance manufacturer's guidance if the proxied requests don't contain these headers when they reach the app.</span></span>

<span data-ttu-id="e6239-137">Předané hlavičky Middleware [výchozí nastavení](#forwarded-headers-middleware-options) lze nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="e6239-137">Forwarded Headers Middleware [default settings](#forwarded-headers-middleware-options) can be configured.</span></span> <span data-ttu-id="e6239-138">Výchozí nastavení je:</span><span class="sxs-lookup"><span data-stu-id="e6239-138">The default settings are:</span></span>

* <span data-ttu-id="e6239-139">Existuje pouze *jeden proxy* mezi zdroji požadavků a aplikace.</span><span class="sxs-lookup"><span data-stu-id="e6239-139">There is only *one proxy* between the app and the source of the requests.</span></span>
* <span data-ttu-id="e6239-140">Pouze adresy zpětné smyčky jsou konfigurovány pro známé proxy servery a známé sítě.</span><span class="sxs-lookup"><span data-stu-id="e6239-140">Only loopback addresses are configured for known proxies and known networks.</span></span>

## <a name="iisiis-express-and-aspnet-core-module"></a><span data-ttu-id="e6239-141">Služby IIS/IIS Express a ASP.NET Core modulu</span><span class="sxs-lookup"><span data-stu-id="e6239-141">IIS/IIS Express and ASP.NET Core Module</span></span>

<span data-ttu-id="e6239-142">Přesměrovaná hlavičky middlewaru je standardně middlewarem integrační služby IIS při spuštění aplikace za služby IIS a ASP.NET Core modulu.</span><span class="sxs-lookup"><span data-stu-id="e6239-142">Forwarded Headers Middleware is enabled by default by IIS Integration Middleware when the app is run behind IIS and the ASP.NET Core Module.</span></span> <span data-ttu-id="e6239-143">Přesměrovaná Middleware hlavičky se aktivuje spuštění první v middlewaru v řadě s konfigurací s omezeným přístupem konkrétní modul ASP.NET Core z důvodu bezpečnosti vztah důvěryhodnosti s přesměrovaná hlavičky (například [falšování adresy IP](https://www.iplocation.net/ip-spoofing)).</span><span class="sxs-lookup"><span data-stu-id="e6239-143">Forwarded Headers Middleware is activated to run first in the middleware pipeline with a restricted configuration specific to the ASP.NET Core Module due to trust concerns with forwarded headers (for example, [IP spoofing](https://www.iplocation.net/ip-spoofing)).</span></span> <span data-ttu-id="e6239-144">Middleware je nakonfigurovaný pro předávání `X-Forwarded-For` a `X-Forwarded-Proto` hlavičky a je omezen na jednom localhost proxy.</span><span class="sxs-lookup"><span data-stu-id="e6239-144">The middleware is configured to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers and is restricted to a single localhost proxy.</span></span> <span data-ttu-id="e6239-145">Pokud je vyžadována další konfigurace, najdete v článku [možnosti předávat middlewaru hlavičky](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="e6239-145">If additional configuration is required, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>

## <a name="other-proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="e6239-146">Další proxy serveru a scénáře pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="e6239-146">Other proxy server and load balancer scenarios</span></span>

<span data-ttu-id="e6239-147">Mimo pomocí middlewaru integrační služby IIS, není ve výchozím nastavení povoleno předávat Middleware hlavičky.</span><span class="sxs-lookup"><span data-stu-id="e6239-147">Outside of using IIS Integration Middleware, Forwarded Headers Middleware isn't enabled by default.</span></span> <span data-ttu-id="e6239-148">Přesměrovaná Middleware hlavičky musí být povolen pro aplikaci na proces předávaných záhlaví s [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="e6239-148">Forwarded Headers Middleware must be enabled for an app to process forwarded headers with [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders).</span></span> <span data-ttu-id="e6239-149">Po povolení middleware, pokud žádné [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) jsou určené pro middleware, výchozí [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) jsou [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="e6239-149">After enabling the middleware if no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span>

<span data-ttu-id="e6239-150">Konfigurace middlewaru s [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) předávat `X-Forwarded-For` a `X-Forwarded-Proto` hlavičky v `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e6239-150">Configure the middleware with [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e6239-151">Vyvolání [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metoda v `Startup.Configure` před voláním jiné middleware:</span><span class="sxs-lookup"><span data-stu-id="e6239-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling other middleware:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    
    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = 
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();
    // In ASP.NET Core 1.x, replace the following line with: app.UseIdentity();
    app.UseAuthentication();
    app.UseMvc();
}
```

> [!NOTE]
> <span data-ttu-id="e6239-152">Pokud žádné [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) jsou určené v `Startup.ConfigureServices` nebo přímo do metody rozšíření s [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), výchozí jsou hlavičky předávat [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="e6239-152">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified in `Startup.ConfigureServices` or directly to the extension method with [UseForwardedHeaders(IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), the default headers to forward are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> <span data-ttu-id="e6239-153">[ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) vlastnost musí být nakonfigurované hlavičky k předávání.</span><span class="sxs-lookup"><span data-stu-id="e6239-153">The [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) property must be configured with the headers to forward.</span></span>

## <a name="forwarded-headers-middleware-options"></a><span data-ttu-id="e6239-154">Přesměrovaná možnosti middlewaru hlavičky</span><span class="sxs-lookup"><span data-stu-id="e6239-154">Forwarded Headers Middleware options</span></span>

<span data-ttu-id="e6239-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) řídí chování middlewaru předávaných hlavičky:</span><span class="sxs-lookup"><span data-stu-id="e6239-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) control the behavior of the Forwarded Headers Middleware:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

| <span data-ttu-id="e6239-156">Možnost</span><span class="sxs-lookup"><span data-stu-id="e6239-156">Option</span></span> | <span data-ttu-id="e6239-157">Popis</span><span class="sxs-lookup"><span data-stu-id="e6239-157">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="e6239-158">ForwardedForHeaderName</span><span class="sxs-lookup"><span data-stu-id="e6239-158">ForwardedForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | <span data-ttu-id="e6239-159">Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).</span><span class="sxs-lookup"><span data-stu-id="e6239-159">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).</span></span><br><br><span data-ttu-id="e6239-160">Výchozí hodnota je `X-Forwarded-For`.</span><span class="sxs-lookup"><span data-stu-id="e6239-160">The default is `X-Forwarded-For`.</span></span> |
| [<span data-ttu-id="e6239-161">ForwardedHeaders</span><span class="sxs-lookup"><span data-stu-id="e6239-161">ForwardedHeaders</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | <span data-ttu-id="e6239-162">Určuje, které servery pro předávání, měla by být zpracována.</span><span class="sxs-lookup"><span data-stu-id="e6239-162">Identifies which forwarders should be processed.</span></span> <span data-ttu-id="e6239-163">Najdete v článku [ForwardedHeaders výčtu](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) pro seznam polí, které se vztahují.</span><span class="sxs-lookup"><span data-stu-id="e6239-163">See the [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) for the list of fields that apply.</span></span> <span data-ttu-id="e6239-164">Typické hodnoty přiřazený k této vlastnosti jsou <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.</span><span class="sxs-lookup"><span data-stu-id="e6239-164">Typical values assigned to this property are <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.</span></span><br><br><span data-ttu-id="e6239-165">Výchozí hodnota je [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="e6239-165">The default value is [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> |
| [<span data-ttu-id="e6239-166">ForwardedHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="e6239-166">ForwardedHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | <span data-ttu-id="e6239-167">Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).</span><span class="sxs-lookup"><span data-stu-id="e6239-167">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).</span></span><br><br><span data-ttu-id="e6239-168">Výchozí hodnota je `X-Forwarded-Host`.</span><span class="sxs-lookup"><span data-stu-id="e6239-168">The default is `X-Forwarded-Host`.</span></span> |
| [<span data-ttu-id="e6239-169">ForwardedProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="e6239-169">ForwardedProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | <span data-ttu-id="e6239-170">Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).</span><span class="sxs-lookup"><span data-stu-id="e6239-170">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).</span></span><br><br><span data-ttu-id="e6239-171">Výchozí hodnota je `X-Forwarded-Proto`.</span><span class="sxs-lookup"><span data-stu-id="e6239-171">The default is `X-Forwarded-Proto`.</span></span> |
| [<span data-ttu-id="e6239-172">ForwardLimit</span><span class="sxs-lookup"><span data-stu-id="e6239-172">ForwardLimit</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | <span data-ttu-id="e6239-173">Omezuje počet položek v hlavičkách, které jsou zpracovány.</span><span class="sxs-lookup"><span data-stu-id="e6239-173">Limits the number of entries in the headers that are processed.</span></span> <span data-ttu-id="e6239-174">Nastavte na `null` zakázat tento limit, ale to lze provádět pouze pokud `KnownProxies` nebo `KnownNetworks` jsou nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="e6239-174">Set to `null` to disable the limit, but this should only be done if `KnownProxies` or `KnownNetworks` are configured.</span></span><br><br><span data-ttu-id="e6239-175">Výchozí hodnota je 1.</span><span class="sxs-lookup"><span data-stu-id="e6239-175">The default is 1.</span></span> |
| [<span data-ttu-id="e6239-176">KnownNetworks</span><span class="sxs-lookup"><span data-stu-id="e6239-176">KnownNetworks</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | <span data-ttu-id="e6239-177">Rozsahy známé serverů proxy tak, aby přijímal přesměrovaná hlaviček z adres.</span><span class="sxs-lookup"><span data-stu-id="e6239-177">Address ranges of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="e6239-178">Zadejte rozsahy IP adres pomocí notace CIDR (Classless Interdomain Routing) zápisu.</span><span class="sxs-lookup"><span data-stu-id="e6239-178">Provide IP ranges using Classless Interdomain Routing (CIDR) notation.</span></span><br><br><span data-ttu-id="e6239-179">Výchozí hodnota je [IList](/dotnet/api/system.collections.generic.ilist-1)\<[třídu IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> obsahující jeden záznam pro `IPAddress.Loopback`.</span><span class="sxs-lookup"><span data-stu-id="e6239-179">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> containing a single entry for `IPAddress.Loopback`.</span></span> |
| [<span data-ttu-id="e6239-180">KnownProxies</span><span class="sxs-lookup"><span data-stu-id="e6239-180">KnownProxies</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | <span data-ttu-id="e6239-181">Adresy známé proxy tak, aby přijímal přesměrovaná hlaviček z.</span><span class="sxs-lookup"><span data-stu-id="e6239-181">Addresses of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="e6239-182">Použití `KnownProxies` nastavit přesnou adresu IP odpovídá.</span><span class="sxs-lookup"><span data-stu-id="e6239-182">Use `KnownProxies` to specify exact IP address matches.</span></span><br><br><span data-ttu-id="e6239-183">Výchozí hodnota je [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> obsahující jeden záznam pro `IPAddress.IPv6Loopback`.</span><span class="sxs-lookup"><span data-stu-id="e6239-183">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> containing a single entry for `IPAddress.IPv6Loopback`.</span></span> |
| [<span data-ttu-id="e6239-184">OriginalForHeaderName</span><span class="sxs-lookup"><span data-stu-id="e6239-184">OriginalForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | <span data-ttu-id="e6239-185">Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).</span><span class="sxs-lookup"><span data-stu-id="e6239-185">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).</span></span><br><br><span data-ttu-id="e6239-186">Výchozí hodnota je `X-Original-For`.</span><span class="sxs-lookup"><span data-stu-id="e6239-186">The default is `X-Original-For`.</span></span> |
| [<span data-ttu-id="e6239-187">OriginalHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="e6239-187">OriginalHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | <span data-ttu-id="e6239-188">Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).</span><span class="sxs-lookup"><span data-stu-id="e6239-188">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).</span></span><br><br><span data-ttu-id="e6239-189">Výchozí hodnota je `X-Original-Host`.</span><span class="sxs-lookup"><span data-stu-id="e6239-189">The default is `X-Original-Host`.</span></span> |
| [<span data-ttu-id="e6239-190">OriginalProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="e6239-190">OriginalProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | <span data-ttu-id="e6239-191">Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).</span><span class="sxs-lookup"><span data-stu-id="e6239-191">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).</span></span><br><br><span data-ttu-id="e6239-192">Výchozí hodnota je `X-Original-Proto`.</span><span class="sxs-lookup"><span data-stu-id="e6239-192">The default is `X-Original-Proto`.</span></span> |
| [<span data-ttu-id="e6239-193">RequireHeaderSymmetry</span><span class="sxs-lookup"><span data-stu-id="e6239-193">RequireHeaderSymmetry</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | <span data-ttu-id="e6239-194">Vyžadovat počet hodnot hlavičky byly synchronizované mezi [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) zpracovává.</span><span class="sxs-lookup"><span data-stu-id="e6239-194">Require the number of header values to be in sync between the [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) being processed.</span></span><br><br><span data-ttu-id="e6239-195">Výchozí hodnota v ASP.NET Core je 1.x `true`.</span><span class="sxs-lookup"><span data-stu-id="e6239-195">The default in ASP.NET Core 1.x is `true`.</span></span> <span data-ttu-id="e6239-196">Výchozí hodnota v technologii ASP.NET Core 2.0 nebo novější je `false`.</span><span class="sxs-lookup"><span data-stu-id="e6239-196">The default in ASP.NET Core 2.0 or later is `false`.</span></span> |

## <a name="scenarios-and-use-cases"></a><span data-ttu-id="e6239-197">Scénáře a případy použití</span><span class="sxs-lookup"><span data-stu-id="e6239-197">Scenarios and use cases</span></span>

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a><span data-ttu-id="e6239-198">Pokud není možné přidat přesměrovaná hlavičky a všechny požadavky jsou bezpečné</span><span class="sxs-lookup"><span data-stu-id="e6239-198">When it isn't possible to add forwarded headers and all requests are secure</span></span>

<span data-ttu-id="e6239-199">V některých případech nemusí být možné přidat přesměrovaná hlavičky na požadavky směrovány přes proxy server na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e6239-199">In some cases, it might not be possible to add forwarded headers to the requests proxied to the app.</span></span> <span data-ttu-id="e6239-200">Pokud proxy server je vynucení, že jsou všechny veřejné externí požadavky HTTPS, schéma můžete ručně nastavit `Startup.Configure` před použitím jakéhokoli typu middleware:</span><span class="sxs-lookup"><span data-stu-id="e6239-200">If the proxy is enforcing that all public external requests are HTTPS, the scheme can be manually set in `Startup.Configure` before using any type of middleware:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

<span data-ttu-id="e6239-201">Tento kód lze zakázat pomocí proměnné prostředí nebo jiné nastavení konfigurace v vývoj nebo pracovní prostředí.</span><span class="sxs-lookup"><span data-stu-id="e6239-201">This code can be disabled with an environment variable or other configuration setting in a development or staging environment.</span></span>

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a><span data-ttu-id="e6239-202">Řešit základ cesty a proxy servery, které změní Cesta požadavku</span><span class="sxs-lookup"><span data-stu-id="e6239-202">Deal with path base and proxies that change the request path</span></span>

<span data-ttu-id="e6239-203">Některé servery proxy předat cesta beze změn, ale k aplikaci základní cesta, která měla by být odebrána, aby směrování funguje správně.</span><span class="sxs-lookup"><span data-stu-id="e6239-203">Some proxies pass the path intact but with an app base path that should be removed so that routing works properly.</span></span> <span data-ttu-id="e6239-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) middleware rozdělí cestu do [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) a základní cesty aplikace do [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).</span><span class="sxs-lookup"><span data-stu-id="e6239-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) middleware splits the path into [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) and the app base path into [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).</span></span>

<span data-ttu-id="e6239-205">Pokud `/foo` je základní cesty aplikace pro cestu k proxy serveru předány jako `/foo/api/1`, nastaví middleware `Request.PathBase` k `/foo` a `Request.Path` k `/api/1` pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="e6239-205">If `/foo` is the app base path for a proxy path passed as `/foo/api/1`, the middleware sets `Request.PathBase` to `/foo` and `Request.Path` to `/api/1` with the following command:</span></span>

```csharp
app.UsePathBase("/foo");
```

<span data-ttu-id="e6239-206">Původní cestu a základ cesty budou znovu, když middleware nebude volán znovu pozpátku.</span><span class="sxs-lookup"><span data-stu-id="e6239-206">The original path and path base are reapplied when the middleware is called again in reverse.</span></span> <span data-ttu-id="e6239-207">Další informace o pořadí zpracování middleware najdete v tématu [Middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="e6239-207">For more information on middleware order processing, see [Middleware](xref:fundamentals/middleware/index).</span></span>

<span data-ttu-id="e6239-208">Pokud proxy server ořízne cestu (například předávání `/foo/api/1` k `/api/1`), opravte přesměruje a propojuje nastavením žádosti [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) vlastnost:</span><span class="sxs-lookup"><span data-stu-id="e6239-208">If the proxy trims the path (for example, forwarding `/foo/api/1` to `/api/1`), fix redirects and links by setting the request's [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) property:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

<span data-ttu-id="e6239-209">Pokud proxy server je přidání data cesty, zahodit část této cesty pomocí opravit přesměrování a odkazy [StartsWithSegments (PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) a přiřazení k [cesta](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) vlastnost:</span><span class="sxs-lookup"><span data-stu-id="e6239-209">If the proxy is adding path data, discard part of the path to fix redirects and links by using [StartsWithSegments(PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) and assigning to the [Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) property:</span></span>

```csharp
app.Use((context, next) =>
{
    if (context.Request.Path.StartsWithSegments("/foo", out var remainder))
    {
        context.Request.Path = remainder;
    }

    return next();
});
```

## <a name="troubleshoot"></a><span data-ttu-id="e6239-210">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="e6239-210">Troubleshoot</span></span>

<span data-ttu-id="e6239-211">Pokud hlavičky nejsou dál podle očekávání, povolit [protokolování](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="e6239-211">When headers aren't forwarded as expected, enable [logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="e6239-212">Pokud protokoly neposkytují dost informací k vyřešení tohoto problému, výčet hlavičky žádosti přijaté serverem.</span><span class="sxs-lookup"><span data-stu-id="e6239-212">If the logs don't provide sufficient information to troubleshoot the problem, enumerate the request headers received by the server.</span></span> <span data-ttu-id="e6239-213">Hlavičky může být zapsán do odpovědi aplikace pomocí middlewaru vložených:</span><span class="sxs-lookup"><span data-stu-id="e6239-213">The headers can be written to an app response using inline middleware:</span></span>

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.Run(async (context) =>
    {
        context.Response.ContentType = "text/plain";

        // Request method, scheme, and path
        await context.Response.WriteAsync(
            $"Request Method: {context.Request.Method}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Scheme: {context.Request.Scheme}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Path: {context.Request.Path}{Environment.NewLine}");

        // Headers
        await context.Response.WriteAsync($"Request Headers:{Environment.NewLine}");

        foreach (var header in context.Request.Headers)
        {
            await context.Response.WriteAsync($"{header.Key}: " +
                $"{header.Value}{Environment.NewLine}");
        }

        await context.Response.WriteAsync(Environment.NewLine);

        // Connection: RemoteIp
        await context.Response.WriteAsync(
            $"Request RemoteIp: {context.Connection.RemoteIpAddress}");
    }
}
```

<span data-ttu-id="e6239-214">Ujistěte se, že X - předávaných-\* hlavičky jsou přijaté serverem s očekávaných hodnot.</span><span class="sxs-lookup"><span data-stu-id="e6239-214">Ensure that the X-Forwarded-\* headers are received by the server with the expected values.</span></span> <span data-ttu-id="e6239-215">Pokud existuje více hodnot v danou hlavičku, poznamenejte si hlavičky procesy předávaných Middleware hlavičky v obráceném pořadí zprava doleva.</span><span class="sxs-lookup"><span data-stu-id="e6239-215">If there are multiple values in a given header, note Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span>

<span data-ttu-id="e6239-216">Původní vzdálené IP žádosti se musí shodovat položku v `KnownProxies` nebo `KnownNetworks` uvádí předtím, než X-předávaných-pro je zpracován.</span><span class="sxs-lookup"><span data-stu-id="e6239-216">The request's original remote IP must match an entry in the `KnownProxies` or `KnownNetworks` lists before X-Forwarded-For is processed.</span></span> <span data-ttu-id="e6239-217">Toto nastavení omezuje záhlaví falšování identity není přijetím z nedůvěryhodné proxy serverů pro předávání.</span><span class="sxs-lookup"><span data-stu-id="e6239-217">This limits header spoofing by not accepting forwarders from untrusted proxies.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e6239-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e6239-218">Additional resources</span></span>

* [<span data-ttu-id="e6239-219">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core chyba zabezpečení zvýšení oprávnění</span><span class="sxs-lookup"><span data-stu-id="e6239-219">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Elevation Of Privilege Vulnerability</span></span>](https://github.com/aspnet/Announcements/issues/295)
