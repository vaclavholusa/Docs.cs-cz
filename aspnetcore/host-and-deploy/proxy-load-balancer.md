---
title: Konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení
author: guardrex
description: Další informace o konfiguraci pro aplikace, které jsou hostovány za proxy servery a požadovat služby Vyrovnávání zatížení, které často skrývat důležité informace.
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: 1797962d6eada9c48b31cd94e2c7481380301a0d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276773"
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>Konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení

Podle [Luke Latham](https://github.com/guardrex) a [Ross Jan](https://github.com/Tratcher)

Doporučenou konfiguraci pro jádro ASP.NET je aplikace hostované pomocí modulu jádra IIS/ASP.NET, Nginx nebo Apache. Proxy servery, nástroje pro vyrovnávání zatížení a jiných síťových zařízení často skrývat informace o požadavku předtím, než dorazí do aplikace:

* Pokud požadavky HTTPS jsou směrovány přes proxy server pomocí protokolu HTTP, původní schématu (HTTPS) dojde ke ztrátě a musí být přeposílán v hlavičce.
* Protože aplikace obdrží žádost z proxy serveru a na Internetu nebo podnikové síti není true zdroj, musí v hlavičce předají zdrojovou IP adresu klienta.

Tyto informace může být důležité v zpracování žádostí, například v přesměrování, ověřování, generování odkazů, vyhodnocení zásad a informace o zeměpisné poloze klienta.

## <a name="forwarded-headers"></a>Přesměrovaná hlavičky

Podle konvence proxy předávat informace v hlavičkách protokolu HTTP.

| Záhlaví | Popis |
| ------ | ----------- |
| X-předávaných pro | Obsahuje informace o klientovi, který spustil požadavků a následné proxy v řetězu serverů proxy. Tento parametr může obsahovat IP adresy (a volitelně čísla portů). V řetězu proxy servery určuje první parametr klienta, kde byl nejprve požadavek. Postupujte podle následujících proxy identifikátory. Poslední proxy server v řetězu není v seznamu parametrů. Poslední proxy IP adresu a volitelně číslo portu, jsou k dispozici jako vzdálené IP adresy v přenosové vrstvě. |
| X předávaných Proto | Hodnota původní schématu (HTTP či HTTPS). Hodnota může být také seznam schémat, pokud požadavek projde více proxy serverů. |
| X předávaných hostitele | Původní hodnotu pole hlavičky hostitele. Obvykle proxy Neupravovat hlavičku hostitele. V tématu [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) informace o zvýšení oprávnění ohrožení zabezpečení, která má vliv na systémy, kde není ověření proxy serveru nebo hlavičky hostitele restict známé dobré hodnoty. |

Middleware hlavičky předávány z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) balíčků, čte tyto hlavičky a vyplní přidružené pole na [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

Middleware aktualizace:

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; nastavit pomocí `X-Forwarded-For` hodnotu hlavičky. Další nastavení mají vliv jak middleware nastaví `RemoteIpAddress`. Podrobnosti najdete v tématu [možnosti předávat middlewaru hlavičky](#forwarded-headers-middleware-options).
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; nastavit pomocí `X-Forwarded-Proto` hodnotu hlavičky.
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; nastavit pomocí `X-Forwarded-Host` hodnotu hlavičky.

Všimněte si, že ne všechna síťová zařízení přidat `X-Forwarded-For` a `X-Forwarded-Proto` hlavičky bez další konfigurace. Vyhledejte pokyny výrobce zařízení, pokud směrovány přes proxy server požadavky nesmí obsahovat tyto hlavičky po uplynutí aplikace.

Předané hlavičky Middleware [výchozí nastavení](#forwarded-headers-middleware-options) lze nakonfigurovat. Výchozí nastavení je:

* Existuje pouze *jeden proxy* mezi zdroji požadavků a aplikace.
* Pouze adresy zpětné smyčky jsou konfigurovány pro známé proxy servery a známé sítě.

## <a name="iisiis-express-and-aspnet-core-module"></a>Služby IIS/IIS Express a ASP.NET Core modulu

Přesměrovaná hlavičky middlewaru je standardně middlewarem integrační služby IIS při spuštění aplikace za služby IIS a ASP.NET Core modulu. Přesměrovaná Middleware hlavičky se aktivuje spuštění první v middlewaru v řadě s konfigurací s omezeným přístupem konkrétní modul ASP.NET Core z důvodu bezpečnosti vztah důvěryhodnosti s přesměrovaná hlavičky (například [falšování adresy IP](https://www.iplocation.net/ip-spoofing)). Middleware je nakonfigurovaný pro předávání `X-Forwarded-For` a `X-Forwarded-Proto` hlavičky a je omezen na jednom localhost proxy. Pokud je vyžadována další konfigurace, najdete v článku [možnosti předávat middlewaru hlavičky](#forwarded-headers-middleware-options).

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Další proxy serveru a scénáře pro vyrovnávání zatížení

Mimo pomocí middlewaru integrační služby IIS, není ve výchozím nastavení povoleno předávat Middleware hlavičky. Přesměrovaná Middleware hlavičky musí být povolen pro aplikaci na proces předávaných záhlaví s [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders). Po povolení middleware, pokud žádné [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) jsou určené pro middleware, výchozí [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) jsou [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).

Konfigurace middlewaru s [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) předávat `X-Forwarded-For` a `X-Forwarded-Proto` hlavičky v `Startup.ConfigureServices`. Vyvolání [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metoda v `Startup.Configure` před voláním jiné middleware:

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
> Pokud žádné [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) jsou určené v `Startup.ConfigureServices` nebo přímo do metody rozšíření s [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), výchozí jsou hlavičky předávat [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) vlastnost musí být nakonfigurované hlavičky k předávání.

## <a name="nginx-configuration"></a>Konfigurace Nginx

Předávání `X-Forwarded-For` a `X-Forwarded-Proto` hlavičky, najdete v části [hostitele v systému Linux s Nginx: Konfigurace Nginx](xref:host-and-deploy/linux-nginx#configure-nginx). Další informace najdete v tématu [NGINX: pomocí hlavičku předané](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/).

## <a name="apache-configuration"></a>Apache konfigurace

`X-Forwarded-For` je automaticky přidán (najdete v části [Apache modulu mod_proxy: Reverse záhlaví požadavku Proxy](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers)). Informace o tom, jak předávat `X-Forwarded-Proto` záhlaví, najdete v části [hostitele v systému Linux s Apache: Konfigurace Apache](xref:host-and-deploy/linux-apache#configure-apache).

## <a name="forwarded-headers-middleware-options"></a>Přesměrovaná možnosti middlewaru hlavičky

[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) řídí chování middlewaru předávaných hlavičky:

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

::: moniker range="<= aspnetcore-2.0"
| Možnost | Popis |
| ------ | ----------- |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).<br><br>Výchozí hodnota je `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Určuje, které servery pro předávání, měla by být zpracována. Najdete v článku [ForwardedHeaders výčtu](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) pro seznam polí, které se vztahují. Typické hodnoty přiřazený k této vlastnosti jsou <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>Výchozí hodnota je [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).<br><br>Výchozí hodnota je `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).<br><br>Výchozí hodnota je `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Omezuje počet položek v hlavičkách, které jsou zpracovány. Nastavte na `null` zakázat tento limit, ale to lze provádět pouze pokud `KnownProxies` nebo `KnownNetworks` jsou nakonfigurované.<br><br>Výchozí hodnota je 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Rozsahy známé serverů proxy tak, aby přijímal přesměrovaná hlaviček z adres. Zadejte rozsahy IP adres pomocí notace CIDR (Classless Interdomain Routing) zápisu.<br><br>Výchozí hodnota je [IList](/dotnet/api/system.collections.generic.ilist-1)\<[třídu IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> obsahující jeden záznam pro `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Adresy známé proxy tak, aby přijímal přesměrovaná hlaviček z. Použití `KnownProxies` nastavit přesnou adresu IP odpovídá.<br><br>Výchozí hodnota je [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> obsahující jeden záznam pro `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>Výchozí hodnota je `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>Výchozí hodnota je `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>Výchozí hodnota je `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Vyžadovat počet hodnot hlavičky byly synchronizované mezi [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) zpracovává.<br><br>Výchozí hodnota v ASP.NET Core je 1.x `true`. Výchozí hodnota v technologii ASP.NET Core 2.0 nebo novější je `false`. |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| Možnost | Popis |
| ------ | ----------- |
| AllowedHosts | Omezuje hostitelů pomocí `X-Forwarded-Host` hlavičky k zadanými hodnotami.<ul><li>Porovnání hodnot pomocí pořadí ignorovat případu.</li><li>Čísla portů musí být vyloučeny.</li><li>Pokud je seznam prázdný, jsou povoleny všechny hostitele.</li><li>Nejvyšší úrovně zástupný znak `*` umožňuje všichni hostitelé není prázdný.</li><li>Subdomény zástupné znaky jsou povolené, ale kořenové domény se neshodují. Například `*.contoso.com` odpovídá subdoméně `foo.contoso.com` , ale není kořenová doména `contoso.com`.</li><li>Názvy hostitelů ve formátu Unicode jsou povolené, ale jsou převedeny na [Punycode](https://tools.ietf.org/html/rfc3492) k porovnání.</li><li>[Adresy IPv6](https://tools.ietf.org/html/rfc4291) musí obsahovat ohraničujícího závorky a být ve [konvenční formuláře](https://tools.ietf.org/html/rfc4291#section-2.2) (například `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`). IPv6 adresy nejsou zvláštní použita ke kontrole rovnosti logické mezi různými formáty a žádné kanonizace se provádí.</li><li>Selhání omezit povolený hostitele může útočníkovi umožnit zfalšovat odkazy vygenerované službou.</li></ul>Výchozí hodnota je prázdná [IList\<řetězec >](/dotnet/api/system.collections.generic.ilist-1). |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).<br><br>Výchozí hodnota je `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Určuje, které servery pro předávání, měla by být zpracována. Najdete v článku [ForwardedHeaders výčtu](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) pro seznam polí, které se vztahují. Typické hodnoty přiřazený k této vlastnosti jsou <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>Výchozí hodnota je [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).<br><br>Výchozí hodnota je `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).<br><br>Výchozí hodnota je `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Omezuje počet položek v hlavičkách, které jsou zpracovány. Nastavte na `null` zakázat tento limit, ale to lze provádět pouze pokud `KnownProxies` nebo `KnownNetworks` jsou nakonfigurované.<br><br>Výchozí hodnota je 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Rozsahy známé serverů proxy tak, aby přijímal přesměrovaná hlaviček z adres. Zadejte rozsahy IP adres pomocí notace CIDR (Classless Interdomain Routing) zápisu.<br><br>Výchozí hodnota je [IList](/dotnet/api/system.collections.generic.ilist-1)\<[třídu IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> obsahující jeden záznam pro `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Adresy známé proxy tak, aby přijímal přesměrovaná hlaviček z. Použití `KnownProxies` nastavit přesnou adresu IP odpovídá.<br><br>Výchozí hodnota je [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> obsahující jeden záznam pro `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>Výchozí hodnota je `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>Výchozí hodnota je `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Použít header této vlastnosti místo určenému [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>Výchozí hodnota je `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Vyžadovat počet hodnot hlavičky byly synchronizované mezi [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) zpracovává.<br><br>Výchozí hodnota v ASP.NET Core je 1.x `true`. Výchozí hodnota v technologii ASP.NET Core 2.0 nebo novější je `false`. |
::: moniker-end

## <a name="scenarios-and-use-cases"></a>Scénáře a případy použití

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>Pokud není možné přidat přesměrovaná hlavičky a všechny požadavky jsou bezpečné

V některých případech nemusí být možné přidat přesměrovaná hlavičky na požadavky směrovány přes proxy server na aplikaci. Pokud proxy server je vynucení, že jsou všechny veřejné externí požadavky HTTPS, schéma můžete ručně nastavit `Startup.Configure` před použitím jakéhokoli typu middleware:

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

Tento kód lze zakázat pomocí proměnné prostředí nebo jiné nastavení konfigurace v vývoj nebo pracovní prostředí.

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>Řešit základ cesty a proxy servery, které změní Cesta požadavku

Některé servery proxy předat cesta beze změn, ale k aplikaci základní cesta, která měla by být odebrána, aby směrování funguje správně. [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) middleware rozdělí cestu do [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) a základní cesty aplikace do [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).

Pokud `/foo` je základní cesty aplikace pro cestu k proxy serveru předány jako `/foo/api/1`, nastaví middleware `Request.PathBase` k `/foo` a `Request.Path` k `/api/1` pomocí následujícího příkazu:

```csharp
app.UsePathBase("/foo");
```

Původní cestu a základ cesty budou znovu, když middleware nebude volán znovu pozpátku. Další informace o pořadí zpracování middleware najdete v tématu [Middleware](xref:fundamentals/middleware/index).

Pokud proxy server ořízne cestu (například předávání `/foo/api/1` k `/api/1`), opravte přesměruje a propojuje nastavením žádosti [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) vlastnost:

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Pokud proxy server je přidání data cesty, zahodit část této cesty pomocí opravit přesměrování a odkazy [StartsWithSegments (PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) a přiřazení k [cesta](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) vlastnost:

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

## <a name="troubleshoot"></a>Řešení potíží

Pokud hlavičky nejsou dál podle očekávání, povolit [protokolování](xref:fundamentals/logging/index). Pokud protokoly neposkytují dost informací k vyřešení tohoto problému, výčet hlavičky žádosti přijaté serverem. Hlavičky může být zapsán do odpovědi aplikace pomocí middlewaru vložených:

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

Ujistěte se, že X - předávaných-* hlavičky jsou přijaté serverem s očekávaných hodnot. Pokud existuje více hodnot v danou hlavičku, poznamenejte si hlavičky procesy předávaných Middleware hlavičky v obráceném pořadí zprava doleva.

Původní vzdálené IP žádosti se musí shodovat položku v `KnownProxies` nebo `KnownNetworks` uvádí předtím, než X-předávaných-pro je zpracován. Toto nastavení omezuje záhlaví falšování identity není přijetím z nedůvěryhodné proxy serverů pro předávání.

## <a name="additional-resources"></a>Další zdroje

* [Microsoft Security Advisory CVE-2018-0787: ASP.NET Core chyba zabezpečení zvýšení oprávnění](https://github.com/aspnet/Announcements/issues/295)
