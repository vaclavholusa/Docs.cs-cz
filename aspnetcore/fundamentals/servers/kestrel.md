---
title: Implementace serveru webové kestrel v ASP.NET Core
author: rick-anderson
description: Další informace o Kestrel, napříč platformami webový server pro ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/01/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: c11a32aec49f4550471fb1399306fe17f1735a5c
ms.sourcegitcommit: 7211ae2dd702f67d36365831c490d6178c9a46c8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44089883"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="151af-103">Implementace serveru webové kestrel v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="151af-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="151af-104">Podle [Petr Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), a [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="151af-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="151af-105">Kestrel je platformově univerzální [webového serveru pro ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="151af-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="151af-106">Kestrel je webový server, který je obsažen ve výchozím nastavení v šablonách projektů ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="151af-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="151af-107">Kestrel podporuje následující funkce:</span><span class="sxs-lookup"><span data-stu-id="151af-107">Kestrel supports the following features:</span></span>

* <span data-ttu-id="151af-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="151af-108">HTTPS</span></span>
* <span data-ttu-id="151af-109">Neprůhledný upgrade nepoužívá k zapnutí [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="151af-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="151af-110">Soketů systému UNIX pro vysoký výkon za serveru Nginx</span><span class="sxs-lookup"><span data-stu-id="151af-110">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="151af-111">Kestrel se podporuje na všech platformách a verze, které podporuje .NET Core.</span><span class="sxs-lookup"><span data-stu-id="151af-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="151af-112">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="151af-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="151af-113">Kdy použít Kestrel pomocí reverzního proxy serveru</span><span class="sxs-lookup"><span data-stu-id="151af-113">When to use Kestrel with a reverse proxy</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="151af-114">Kestrel můžete použít samostatně nebo se *reverzní proxy server*, jako je například Apache, IIS nebo Nginx.</span><span class="sxs-lookup"><span data-stu-id="151af-114">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="151af-115">Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je na Kestrel po některé předběžného zpracování.</span><span class="sxs-lookup"><span data-stu-id="151af-115">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel komunikuje přímo s Internetu bez reverzní proxy server](kestrel/_static/kestrel-to-internet2.png)

![Kestrel nepřímo komunikuje přes Internet prostřednictvím reverzního proxy serveru, jako je například Apache, IIS nebo Nginx](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="151af-118">Buď konfiguraci&mdash;s nebo bez něj reverzní proxy server&mdash;je platný a podporované konfigurace pro hostování pro ASP.NET Core 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="151af-118">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="151af-119">Pokud aplikace přijímá požadavky jenom z interní sítě, Kestrel lze použít přímo jako server aplikace.</span><span class="sxs-lookup"><span data-stu-id="151af-119">If an app accepts requests only from an internal network, Kestrel can be used directly as the app's server.</span></span>

![Kestrel komunikuje přímo s vaší interní sítě](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="151af-121">Pokud je zveřejnit aplikaci k Internetu, použít službu IIS, serveru Nginx nebo Apache jako *reverzní proxy server*.</span><span class="sxs-lookup"><span data-stu-id="151af-121">If you expose your app to the Internet, use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="151af-122">Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je na Kestrel po některé předběžného zpracování.</span><span class="sxs-lookup"><span data-stu-id="151af-122">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel nepřímo komunikuje přes Internet prostřednictvím reverzního proxy serveru, jako je například Apache, IIS nebo Nginx](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="151af-124">Reverzní proxy je vyžadován pro nasazení hraniční (vystavené pro provoz z Internetu) z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="151af-124">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="151af-125">Verze 1.x Kestrel nemají úplný doplněk obranu před útoky, jako je například omezení počtu souběžných připojení, odpovídající časové limity a omezení velikosti.</span><span class="sxs-lookup"><span data-stu-id="151af-125">The 1.x versions of Kestrel don't have a full complement of defenses against attacks, such as appropriate timeouts, size limits, and concurrent connection limits.</span></span>

::: moniker-end

<span data-ttu-id="151af-126">Scénář reverzního proxy serveru existuje, pokud existuje víc aplikací, které sdílejí stejné IP adresy a portu, které běží na jednom serveru.</span><span class="sxs-lookup"><span data-stu-id="151af-126">A reverse proxy scenario exists when there are multiple apps that share the same IP and port running on a single server.</span></span> <span data-ttu-id="151af-127">Kestrel tento scénář nepodporuje, protože Kestrel nepodporuje sdílení stejné IP adresy a portu mezi více procesy.</span><span class="sxs-lookup"><span data-stu-id="151af-127">Kestrel doesn't support this scenario because Kestrel doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="151af-128">Když Kestrel je nakonfigurovaná k naslouchání na portu, zpracovává Kestrel veškerý síťový provoz na tento port bez ohledu na to, požadavky se hlavička hostitele.</span><span class="sxs-lookup"><span data-stu-id="151af-128">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' host header.</span></span> <span data-ttu-id="151af-129">Reverzní proxy server, který můžete sdílet porty má schopnost Kestrel na jedinečné IP adresy a portu směrování žádostí.</span><span class="sxs-lookup"><span data-stu-id="151af-129">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="151af-130">I v případě reverzního proxy serveru není povinné, pomocí reverzního proxy serveru může být dobrou volbou:</span><span class="sxs-lookup"><span data-stu-id="151af-130">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice:</span></span>

* <span data-ttu-id="151af-131">Může být omezena vystavené veřejné útoku na aplikace, které hostuje.</span><span class="sxs-lookup"><span data-stu-id="151af-131">It can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="151af-132">Poskytuje další úroveň ochrany a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="151af-132">It provides an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="151af-133">Integrace může lépe se stávající infrastrukturou.</span><span class="sxs-lookup"><span data-stu-id="151af-133">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="151af-134">Zjednodušuje Vyrovnávání zatížení a konfigurace protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="151af-134">It simplifies load balancing and SSL configuration.</span></span> <span data-ttu-id="151af-135">Reverzní proxy server vyžaduje certifikát SSL, a tento server může komunikovat s aplikačních serverů v interní síti přes standardní HTTP.</span><span class="sxs-lookup"><span data-stu-id="151af-135">Only the reverse proxy server requires an SSL certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="151af-136">Pokud nepoužíváte reverzního proxy serveru s hostitelem filtrování povolena, [hostitele filtrování](#host-filtering) musí být povolené.</span><span class="sxs-lookup"><span data-stu-id="151af-136">If not using a reverse proxy with host filtering enabled, [host filtering](#host-filtering) must be enabled.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="151af-137">Jak používat Kestrel v aplikacích ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="151af-137">How to use Kestrel in ASP.NET Core apps</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="151af-138">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) je součástí balíčku [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="151af-138">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="151af-139">Šablony projektů ASP.NET Core pomocí Kestrel ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="151af-139">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="151af-140">V *Program.cs*, kód volání šablony [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), který volá [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) na pozadí.</span><span class="sxs-lookup"><span data-stu-id="151af-140">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="151af-141">Poskytnout další konfigurace po volání `CreateDefaultBuilder`, použijte `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="151af-141">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
.ConfigureKestrel((context, options) =>
{
    // Set properties and call methods on options
});
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

<span data-ttu-id="151af-142">Poskytnout další konfigurace po volání `CreateDefaultBuilder`, volání [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):</span><span class="sxs-lookup"><span data-stu-id="151af-142">To provide additional configuration after calling `CreateDefaultBuilder`, call [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):</span></span>

```csharp
.UseKestrel(options =>
{
    // Set properties and call methods on options
});
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="151af-143">Nainstalujte [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="151af-143">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="151af-144">Volání [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) rozšiřující metody na [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) v `Main` metodu, zadáte některý [Kestrel možnosti](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) vyžaduje, jak je znázorněno v následující části.</span><span class="sxs-lookup"><span data-stu-id="151af-144">Call the [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) in the `Main` method, specifying any [Kestrel options](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) required, as shown in the next section.</span></span>

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

::: moniker-end

### <a name="kestrel-options"></a><span data-ttu-id="151af-145">Možnosti kestrel</span><span class="sxs-lookup"><span data-stu-id="151af-145">Kestrel options</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="151af-146">Webový server Kestrel má omezení možnosti konfigurace, které jsou obzvláště užitečné v nasazeních s přístupem k Internetu.</span><span class="sxs-lookup"><span data-stu-id="151af-146">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="151af-147">Pár důležitých omezení, které se dají přizpůsobit:</span><span class="sxs-lookup"><span data-stu-id="151af-147">A few important limits that can be customized:</span></span>

* <span data-ttu-id="151af-148">Maximální počet klientských připojení</span><span class="sxs-lookup"><span data-stu-id="151af-148">Maximum client connections</span></span>
* <span data-ttu-id="151af-149">Velikost textu maximální požadavku</span><span class="sxs-lookup"><span data-stu-id="151af-149">Maximum request body size</span></span>
* <span data-ttu-id="151af-150">Minimální požadavek tělo přenosová rychlost</span><span class="sxs-lookup"><span data-stu-id="151af-150">Minimum request body data rate</span></span>

<span data-ttu-id="151af-151">Nastavte na tyto a další omezení [omezení](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) vlastnost [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) třídy.</span><span class="sxs-lookup"><span data-stu-id="151af-151">Set these and other constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="151af-152">`Limits` Vlastnost obsahuje instanci [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) třídy.</span><span class="sxs-lookup"><span data-stu-id="151af-152">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

<span data-ttu-id="151af-153">**Maximální počet klientských připojení**</span><span class="sxs-lookup"><span data-stu-id="151af-153">**Maximum client connections**</span></span>

[<span data-ttu-id="151af-154">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="151af-154">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="151af-155">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="151af-155">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

::: moniker-end

<span data-ttu-id="151af-156">Lze nastavit maximální počet souběžných otevřená připojení TCP pro celou aplikaci s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="151af-156">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
.ConfigureKestrel((context, options) =>
{
    options.Limits.MaxConcurrentConnections = 100;
});
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

::: moniker-end

<span data-ttu-id="151af-157">Neexistuje samostatné limit pro připojení, která se upgradovaly z protokolu HTTP nebo HTTPS na jiné protokol (například v požadavku Websocket).</span><span class="sxs-lookup"><span data-stu-id="151af-157">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="151af-158">Po dokončení upgradu spojení se započítává `MaxConcurrentConnections` limit.</span><span class="sxs-lookup"><span data-stu-id="151af-158">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
.ConfigureKestrel((context, options) =>
{
    options.Limits.MaxConcurrentUpgradedConnections = 100;
});
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="151af-159">Maximální počet připojení je neomezený počet (null) ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="151af-159">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="151af-160">**Velikost textu maximální požadavku**</span><span class="sxs-lookup"><span data-stu-id="151af-160">**Maximum request body size**</span></span>

[<span data-ttu-id="151af-161">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="151af-161">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="151af-162">Výchozí velikost těla maximální požadavku je 30,000,000 bajtů, což je přibližně 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="151af-162">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="151af-163">Doporučený postup, chcete-li přepsat omezení v aplikaci ASP.NET Core MVC je použít [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) atributu na metodu akce:</span><span class="sxs-lookup"><span data-stu-id="151af-163">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

::: moniker-end

<span data-ttu-id="151af-164">Tady je příklad, který ukazuje, jak nakonfigurovat omezení pro aplikace u každého požadavku:</span><span class="sxs-lookup"><span data-stu-id="151af-164">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
.ConfigureKestrel((context, options) =>
{
    options.Limits.MaxRequestBodySize = 10 * 1024;
});
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="151af-165">Můžete přepsat nastavení pro konkrétní žádost a v middlewaru:</span><span class="sxs-lookup"><span data-stu-id="151af-165">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="151af-166">Pokud se pokusíte nakonfigurovat limit na vyžádání po spuštění aplikace k přečtení požadavku, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="151af-166">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="151af-167">Je `IsReadOnly` vlastnost, která označuje, zda `MaxRequestBodySize` vlastnost je ve stavu jen pro čtení, což znamená, je příliš pozdě Konfigurace limitu.</span><span class="sxs-lookup"><span data-stu-id="151af-167">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="151af-168">**Minimální požadavek tělo přenosová rychlost**</span><span class="sxs-lookup"><span data-stu-id="151af-168">**Minimum request body data rate**</span></span>

[<span data-ttu-id="151af-169">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="151af-169">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="151af-170">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="151af-170">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="151af-171">Kestrel kontroluje každou sekundu, pokud je dat přicházejících u určenou míru v bajtech/sekundu.</span><span class="sxs-lookup"><span data-stu-id="151af-171">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="151af-172">Pokud míra klesne pod minimální, vypršení časového limitu připojení. Období odkladu je množství času, aby Kestrel poskytuje klienta ke zvýšení jeho míra odesílání z až Toto minimum, frekvence není kontrolován během této doby.</span><span class="sxs-lookup"><span data-stu-id="151af-172">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="151af-173">Období odkladu pomáhá předejít, vyřadit připojení, která původně odesílají data s nízkou rychlostí kvůli zpomalit TCP-start.</span><span class="sxs-lookup"><span data-stu-id="151af-173">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="151af-174">Výchozí minimální rychlost je 240 bajtů za sekundu s období odkladu 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="151af-174">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="151af-175">Minimální sazba platí také pro odpověď.</span><span class="sxs-lookup"><span data-stu-id="151af-175">A minimum rate also applies to the response.</span></span> <span data-ttu-id="151af-176">Kód pro nastavení limitu požadavků a omezení odpovědi je stejná s výjimkou s `RequestBody` nebo `Response` v názvech vlastností a interface.</span><span class="sxs-lookup"><span data-stu-id="151af-176">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="151af-177">Tady je příklad, který ukazuje, jak nakonfigurovat minimální datové sazby v *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="151af-177">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

```csharp
.ConfigureKestrel((context, options) =>
{
    options.Limits.MinRequestBodyDataRate =
        new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
    options.Limits.MinResponseDataRate =
        new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
});
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="151af-178">Sazby za žádost můžete nakonfigurovat v middlewaru:</span><span class="sxs-lookup"><span data-stu-id="151af-178">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="151af-179">Informace o dalších možnostech Kestrel a omezení najdete tady:</span><span class="sxs-lookup"><span data-stu-id="151af-179">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="151af-180">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="151af-180">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="151af-181">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="151af-181">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="151af-182">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="151af-182">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="151af-183">Informace o možnostech Kestrel a omezení najdete tady:</span><span class="sxs-lookup"><span data-stu-id="151af-183">For information about Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="151af-184">Třída KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="151af-184">KestrelServerOptions class</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [<span data-ttu-id="151af-185">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="151af-185">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

::: moniker-end

### <a name="endpoint-configuration"></a><span data-ttu-id="151af-186">Konfigurace koncového bodu</span><span class="sxs-lookup"><span data-stu-id="151af-186">Endpoint configuration</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="151af-187">Ve výchozím nastavení, ASP.NET Core váže k `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="151af-187">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="151af-188">Volání [naslouchání](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) nebo [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) metody [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) konfigurace předpony adres URL a portů pro Kestrel.</span><span class="sxs-lookup"><span data-stu-id="151af-188">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span> <span data-ttu-id="151af-189">`UseUrls`, `--urls` argument příkazového řádku, `urls` konfigurační klíč hostitele a `ASPNETCORE_URLS` proměnnou prostředí také pracovní ale mají omezení, později uvedené v této části.</span><span class="sxs-lookup"><span data-stu-id="151af-189">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section.</span></span>

<span data-ttu-id="151af-190">`urls` Konfigurační klíč hostitele musí pocházet z konfigurace hostitele, není konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="151af-190">The `urls` host configuration key must come from the host configuration, not the app configuration.</span></span> <span data-ttu-id="151af-191">Přidávání `urls` klíče a hodnoty *appsettings.json* konfigurace hostitele nemá vliv, protože hostitel je zcela inicializován době konfigurace je pro čtení, z konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="151af-191">Adding a `urls` key and value to *appsettings.json* doesn't affect host configuration because the host is completely initialized by the time the configuration is read from the configuration file.</span></span> <span data-ttu-id="151af-192">Ale `urls` klíče v *appsettings.json* jde použít s [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) na tvůrce hostitele a nakonfigurujte hostitele:</span><span class="sxs-lookup"><span data-stu-id="151af-192">However, a `urls` key in *appsettings.json* can be used with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) on the host builder to configure the host:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="151af-193">Ve výchozím nastavení ASP.NET Core váže na:</span><span class="sxs-lookup"><span data-stu-id="151af-193">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="151af-194">`https://localhost:5001` (Pokud je místní vývojový certifikát k dispozici)</span><span class="sxs-lookup"><span data-stu-id="151af-194">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="151af-195">Certifikát pro vývoj se vytvoří:</span><span class="sxs-lookup"><span data-stu-id="151af-195">A development certificate is created:</span></span>

* <span data-ttu-id="151af-196">Když [.NET Core SDK](/dotnet/core/sdk) je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="151af-196">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="151af-197">[Nástroji dev-certs](xref:aspnetcore-2.1#https) slouží k vytvoření certifikátu.</span><span class="sxs-lookup"><span data-stu-id="151af-197">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="151af-198">Některé prohlížeče vyžadují, abyste udělili explicitní oprávnění pro prohlížeč důvěřovat certifikátům místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="151af-198">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="151af-199">ASP.NET Core 2.1 a vyšší šablony projektů konfigurace aplikací ve výchozím nastavení spouští na protokol HTTPS a zahrnout [přesměrování protokolu HTTPS a HSTS podporují](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="151af-199">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="151af-200">Volání [naslouchání](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) nebo [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) metody [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) konfigurace předpony adres URL a portů pro Kestrel.</span><span class="sxs-lookup"><span data-stu-id="151af-200">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="151af-201">`UseUrls`, `--urls` argument příkazového řádku, `urls` konfigurační klíč hostitele a `ASPNETCORE_URLS` proměnnou prostředí také pracovní ale mají omezení, později uvedené v této části (výchozí certifikát musí být k dispozici pro koncový bod HTTPS Konfigurace).</span><span class="sxs-lookup"><span data-stu-id="151af-201">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="151af-202">ASP.NET Core 2.1 `KestrelServerOptions` konfigurace:</span><span class="sxs-lookup"><span data-stu-id="151af-202">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

<span data-ttu-id="151af-203">**ConfigureEndpointDefaults (akce&lt;ListenOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="151af-203">**ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)**</span></span>  
<span data-ttu-id="151af-204">Určuje konfiguraci `Action` pro spuštění v každý zadaný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="151af-204">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="151af-205">Volání `ConfigureEndpointDefaults` více než jednou nahradí předchozí `Action`s poslední `Action` zadané.</span><span class="sxs-lookup"><span data-stu-id="151af-205">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="151af-206">**ConfigureHttpsDefaults (akce&lt;HttpsConnectionAdapterOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="151af-206">**ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)**</span></span>  
<span data-ttu-id="151af-207">Určuje konfiguraci `Action` ke spuštění pro každý koncový bod HTTPS.</span><span class="sxs-lookup"><span data-stu-id="151af-207">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="151af-208">Volání `ConfigureHttpsDefaults` více než jednou nahradí předchozí `Action`s poslední `Action` zadané.</span><span class="sxs-lookup"><span data-stu-id="151af-208">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="151af-209">**Configure(IConfiguration)**</span><span class="sxs-lookup"><span data-stu-id="151af-209">**Configure(IConfiguration)**</span></span>  
<span data-ttu-id="151af-210">Vytvoří zavaděč konfigurace pro nastavení Kestrel, který přebírá [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) jako vstup.</span><span class="sxs-lookup"><span data-stu-id="151af-210">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="151af-211">Konfigurace musí být určená ke konfiguračnímu oddílu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="151af-211">The configuration must be scoped to the configuration section for Kestrel.</span></span>

<span data-ttu-id="151af-212">**ListenOptions.UseHttps**</span><span class="sxs-lookup"><span data-stu-id="151af-212">**ListenOptions.UseHttps**</span></span>  
<span data-ttu-id="151af-213">Nakonfigurujte Kestrel k používání HTTPS.</span><span class="sxs-lookup"><span data-stu-id="151af-213">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="151af-214">`ListenOptions.UseHttps` rozšíření:</span><span class="sxs-lookup"><span data-stu-id="151af-214">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="151af-215">`UseHttps` &ndash; Nakonfigurujte Kestrel k používání HTTPS pomocí certifikátu výchozí.</span><span class="sxs-lookup"><span data-stu-id="151af-215">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="151af-216">Vyvolá výjimku, pokud je nakonfigurovaný žádný výchozí certifikát.</span><span class="sxs-lookup"><span data-stu-id="151af-216">Throws an exception if no default certificate is configured.</span></span>
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

<span data-ttu-id="151af-217">`ListenOptions.UseHttps` Parametry:</span><span class="sxs-lookup"><span data-stu-id="151af-217">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="151af-218">`filename` je název a cesta k souboru soubor certifikátu, relativní k adresáři, který obsahuje soubory obsahu aplikace.</span><span class="sxs-lookup"><span data-stu-id="151af-218">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="151af-219">`password` je heslo pro přístup k datům certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="151af-219">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="151af-220">`configureOptions` je `Action` ke konfiguraci `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="151af-220">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="151af-221">Vrátí `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="151af-221">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="151af-222">`storeName` je do úložiště certifikátů, ze kterého se má načíst certifikát.</span><span class="sxs-lookup"><span data-stu-id="151af-222">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="151af-223">`subject` je název subjektu certifikátu.</span><span class="sxs-lookup"><span data-stu-id="151af-223">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="151af-224">`allowInvalid` Označuje, pokud neplatné certifikáty by měl být, jako jsou certifikáty podepsané svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="151af-224">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="151af-225">`location` je umístění úložiště pro načtení certifikátu.</span><span class="sxs-lookup"><span data-stu-id="151af-225">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="151af-226">`serverCertificate` je certifikát X.509.</span><span class="sxs-lookup"><span data-stu-id="151af-226">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="151af-227">V produkčním prostředí musí být explicitně nakonfigurován protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="151af-227">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="151af-228">Minimálně je třeba zadat výchozího certifikátu.</span><span class="sxs-lookup"><span data-stu-id="151af-228">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="151af-229">Podporované konfigurace je popsáno dále:</span><span class="sxs-lookup"><span data-stu-id="151af-229">Supported configurations described next:</span></span>

* <span data-ttu-id="151af-230">Žádná konfigurace</span><span class="sxs-lookup"><span data-stu-id="151af-230">No configuration</span></span>
* <span data-ttu-id="151af-231">Nahraďte výchozí certifikát z konfigurace</span><span class="sxs-lookup"><span data-stu-id="151af-231">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="151af-232">Změnit výchozí nastavení v kódu</span><span class="sxs-lookup"><span data-stu-id="151af-232">Change the defaults in code</span></span>

<span data-ttu-id="151af-233">*Žádná konfigurace*</span><span class="sxs-lookup"><span data-stu-id="151af-233">*No configuration*</span></span>

<span data-ttu-id="151af-234">Naslouchá kestrel `http://localhost:5000` a `https://localhost:5001` (Pokud je k dispozici výchozí cert).</span><span class="sxs-lookup"><span data-stu-id="151af-234">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="151af-235">Určení adres URL pomocí:</span><span class="sxs-lookup"><span data-stu-id="151af-235">Specify URLs using the:</span></span>

* <span data-ttu-id="151af-236">`ASPNETCORE_URLS` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="151af-236">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="151af-237">`--urls` argument příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="151af-237">`--urls` command-line argument.</span></span>
* <span data-ttu-id="151af-238">`urls` Konfigurační klíč hostitele.</span><span class="sxs-lookup"><span data-stu-id="151af-238">`urls` host configuration key.</span></span>
* <span data-ttu-id="151af-239">`UseUrls` metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="151af-239">`UseUrls` extension method.</span></span>

<span data-ttu-id="151af-240">Další informace najdete v tématu [adresy URL serveru](xref:fundamentals/host/web-host#server-urls) a [konfigurace přepisování](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="151af-240">For more information, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="151af-241">Hodnota zadaná pomocí těchto přístupů může být jeden nebo více HTTP a HTTPS koncové body (HTTPS Pokud je k dispozici výchozí cert).</span><span class="sxs-lookup"><span data-stu-id="151af-241">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="151af-242">Nakonfigurujte tuto hodnotu jako seznam oddělený středníkem (například `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="151af-242">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="151af-243">*Nahraďte výchozí certifikát z konfigurace*</span><span class="sxs-lookup"><span data-stu-id="151af-243">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="151af-244">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) volání `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` ve výchozím nastavení se načíst konfiguraci Kestrel.</span><span class="sxs-lookup"><span data-stu-id="151af-244">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="151af-245">Schéma konfigurace k nastavení výchozí HTTPS aplikace je k dispozici pro Kestrel.</span><span class="sxs-lookup"><span data-stu-id="151af-245">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="151af-246">Konfigurovat několik koncových bodů, včetně adresy URL a certifikáty, které chcete použít, ze souboru na disku nebo z úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="151af-246">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="151af-247">V následujícím *appsettings.json* příkladu:</span><span class="sxs-lookup"><span data-stu-id="151af-247">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="151af-248">Nastavte **AllowInvalid** k `true` tak, aby povolovala použití neplatné certifikáty (například certifikáty podepsané svým držitelem).</span><span class="sxs-lookup"><span data-stu-id="151af-248">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="151af-249">Libovolný koncový bod HTTPS, který nemá určenou certifikát (**HttpsDefaultCert** v následujícím příkladu) spadne zpět na cert definované v části **certifikáty** > **výchozí**  nebo certifikát pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="151af-249">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="151af-250">O alternativu k použití **cesta** a **heslo** pro jakýkoliv certifikát je uzel a určete certifikát, pomocí polí úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="151af-250">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="151af-251">Například **certifikáty** > **výchozí** certifikát lze zadat jako:</span><span class="sxs-lookup"><span data-stu-id="151af-251">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="151af-252">Schéma poznámek:</span><span class="sxs-lookup"><span data-stu-id="151af-252">Schema notes:</span></span>

* <span data-ttu-id="151af-253">Koncové body názvy jsou malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="151af-253">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="151af-254">Například `HTTPS` a `Https` jsou platné.</span><span class="sxs-lookup"><span data-stu-id="151af-254">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="151af-255">`Url` Parametr je povinný pro každý koncový bod.</span><span class="sxs-lookup"><span data-stu-id="151af-255">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="151af-256">Formát pro tento parametr je stejné jako na nejvyšší úrovni `Urls` parametr konfigurace s výjimkou, že je omezena na jednu hodnotu.</span><span class="sxs-lookup"><span data-stu-id="151af-256">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="151af-257">Nahraďte tyto koncové body jsou definované na nejvyšší úrovni `Urls` místo přidávání k nim.</span><span class="sxs-lookup"><span data-stu-id="151af-257">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="151af-258">Koncové body definované v kódu prostřednictvím `Listen` jsou kumulativní se koncové body definované v konfiguračním oddílu.</span><span class="sxs-lookup"><span data-stu-id="151af-258">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="151af-259">`Certificate` Část je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="151af-259">The `Certificate` section is optional.</span></span> <span data-ttu-id="151af-260">Pokud `Certificate` oddílu není zadán, budou použity výchozí hodnoty definované v předchozích případech.</span><span class="sxs-lookup"><span data-stu-id="151af-260">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="151af-261">Pokud nejsou k dispozici žádné výchozí hodnoty, vyvolá výjimku, server a nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="151af-261">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="151af-262">`Certificate` Části podporuje obě **cesta**&ndash;**heslo** a **subjektu**&ndash;**Store** certifikáty.</span><span class="sxs-lookup"><span data-stu-id="151af-262">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="151af-263">Tímto způsobem lze definovat libovolný počet koncových bodů tak dlouho, dokud není způsobují konflikty portu.</span><span class="sxs-lookup"><span data-stu-id="151af-263">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="151af-264">`options.Configure(context.Configuration.GetSection("Kestrel"))` Vrátí `KestrelConfigurationLoader` s `.Endpoint(string name, options => { })` metodu, která slouží k doplnění nastavení konfigurovaný koncový bod:</span><span class="sxs-lookup"><span data-stu-id="151af-264">`options.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  options.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="151af-265">Můžete také přímý přístup k `KestrelServerOptions.ConfigurationLoader` chcete zachovat iterace na existující zavaděč, jako je ta poskytuje [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="151af-265">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

* <span data-ttu-id="151af-266">Oddíl konfigurace pro každý koncový bod je k dispozici na možnosti v `Endpoint` metodu tak, aby mohou číst vlastní nastavení.</span><span class="sxs-lookup"><span data-stu-id="151af-266">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="151af-267">Více konfigurací může být načten voláním `options.Configure(context.Configuration.GetSection("Kestrel"))` znovu s jinou část.</span><span class="sxs-lookup"><span data-stu-id="151af-267">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="151af-268">Pouze poslední konfigurace se použije, pokud `Load` je explicitně zavolán v předchozích instancí.</span><span class="sxs-lookup"><span data-stu-id="151af-268">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="151af-269">Microsoft.aspnetcore.all nevolá `Load` tak, aby jeho výchozí konfigurační oddíl může být nahrazen.</span><span class="sxs-lookup"><span data-stu-id="151af-269">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="151af-270">`KestrelConfigurationLoader` zrcadlení `Listen` rozhraní API z řady `KestrelServerOptions` jako `Endpoint` přetížení, takže koncové body kódu a konfiguračním může být nakonfigurována na stejném místě.</span><span class="sxs-lookup"><span data-stu-id="151af-270">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="151af-271">Tato přetížení nepoužívejte názvy a využívat pouze výchozí nastavení z konfigurace.</span><span class="sxs-lookup"><span data-stu-id="151af-271">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="151af-272">*Změnit výchozí nastavení v kódu*</span><span class="sxs-lookup"><span data-stu-id="151af-272">*Change the defaults in code*</span></span>

<span data-ttu-id="151af-273">`ConfigureEndpointDefaults` a `ConfigureHttpsDefaults` můžete použít ke změně výchozího nastavení pro `ListenOptions` a `HttpsConnectionAdapterOptions`, včetně přepisuje výchozí certifikát uvedený v předchozím scénáři.</span><span class="sxs-lookup"><span data-stu-id="151af-273">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="151af-274">`ConfigureEndpointDefaults` a `ConfigureHttpsDefaults` by měla být volána před všechny koncové body se konfigurují.</span><span class="sxs-lookup"><span data-stu-id="151af-274">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

<span data-ttu-id="151af-275">*Podpora kestrel SNI*</span><span class="sxs-lookup"><span data-stu-id="151af-275">*Kestrel support for SNI*</span></span>

<span data-ttu-id="151af-276">[Indikace názvu serveru (SNI)](https://tools.ietf.org/html/rfc6066#section-3) lze použít k hostování více domén na stejné IP adrese a portu.</span><span class="sxs-lookup"><span data-stu-id="151af-276">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="151af-277">Pro SNI na funkci klient odešle název hostitele pro zabezpečenou relaci na server během provádění metody handshake TLS server může poskytnout správný certifikát.</span><span class="sxs-lookup"><span data-stu-id="151af-277">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="151af-278">Klient použije certifikát zařízená šifrovanou komunikaci se serverem během zabezpečené relace, který následuje TLS handshake.</span><span class="sxs-lookup"><span data-stu-id="151af-278">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="151af-279">Podporuje SNI přes kestrel `ServerCertificateSelector` zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="151af-279">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="151af-280">Zpětné volání je vyvolat jednou za připojení, aby mohla aplikace ke kontrole názvu hostitele a vyberte příslušný certifikát.</span><span class="sxs-lookup"><span data-stu-id="151af-280">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="151af-281">Podpora SNI vyžaduje:</span><span class="sxs-lookup"><span data-stu-id="151af-281">SNI support requires:</span></span>

* <span data-ttu-id="151af-282">Běží na rozhraní .NET framework `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="151af-282">Running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="151af-283">Na `netcoreapp2.0` a `net461`, zpětné volání vyvolat ale `name` je vždy `null`.</span><span class="sxs-lookup"><span data-stu-id="151af-283">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="151af-284">`name` Je také `null` Pokud klienta neobsahuje hostitelský název parametru v TLS handshake.</span><span class="sxs-lookup"><span data-stu-id="151af-284">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="151af-285">Spustit všechny weby na stejnou instanci Kestrel.</span><span class="sxs-lookup"><span data-stu-id="151af-285">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="151af-286">Kestrel nepodporuje sdílení IP adresu a port ve více instancích bez reverzního proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="151af-286">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

```csharp
WebHost.CreateDefaultBuilder()
    .ConfigureKestrel((context, options) =>
    {
        options.ListenAnyIP(5005, listenOptions =>
        {
            listenOptions.UseHttps(httpsOptions =>
            {
                var localhostCert = CertificateLoader.LoadFromStoreCert(
                    "localhost", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var exampleCert = CertificateLoader.LoadFromStoreCert(
                    "example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var subExampleCert = CertificateLoader.LoadFromStoreCert(
                    "sub.example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var certs = new Dictionary<string, X509Certificate2>(
                    StringComparer.OrdinalIgnoreCase);
                certs["localhost"] = localhostCert;
                certs["example.com"] = exampleCert;
                certs["sub.example.com"] = subExampleCert;

                httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                {
                    if (name != null && certs.TryGetValue(name, out var cert))
                    {
                        return cert;
                    }

                    return exampleCert;
                };
            });
        });
    });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
WebHost.CreateDefaultBuilder()
    .UseKestrel((context, options) =>
    {
        options.ListenAnyIP(5005, listenOptions =>
        {
            listenOptions.UseHttps(httpsOptions =>
            {
                var localhostCert = CertificateLoader.LoadFromStoreCert(
                    "localhost", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var exampleCert = CertificateLoader.LoadFromStoreCert(
                    "example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var subExampleCert = CertificateLoader.LoadFromStoreCert(
                    "sub.example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var certs = new Dictionary<string, X509Certificate2>(
                    StringComparer.OrdinalIgnoreCase);
                certs["localhost"] = localhostCert;
                certs["example.com"] = exampleCert;
                certs["sub.example.com"] = subExampleCert;

                httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                {
                    if (name != null && certs.TryGetValue(name, out var cert))
                    {
                        return cert;
                    }

                    return exampleCert;
                };
            });
        });
    });
```

::: moniker-end

<span data-ttu-id="151af-287">**Vytvoření vazby na soket TCP**</span><span class="sxs-lookup"><span data-stu-id="151af-287">**Bind to a TCP socket**</span></span>

<span data-ttu-id="151af-288">[Naslouchání](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) metoda vytvoří vazbu na soket TCP a umožňuje lambda s možností konfigurace certifikátu SSL:</span><span class="sxs-lookup"><span data-stu-id="151af-288">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits SSL certificate configuration:</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Loopback, 8000);
            options.Listen(IPAddress.Loopback, 8001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="151af-289">Tento příklad konfiguruje SSL pro koncový bod s [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span><span class="sxs-lookup"><span data-stu-id="151af-289">The example configures SSL for an endpoint with [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="151af-290">Nakonfigurujte další nastavení Kestrel pro konkrétní koncové body pomocí stejného rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="151af-290">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

<span data-ttu-id="151af-291">**Vytvoření vazby k Unixovému soketu**</span><span class="sxs-lookup"><span data-stu-id="151af-291">**Bind to a Unix socket**</span></span>

<span data-ttu-id="151af-292">Naslouchání soketu Unix s [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) dosáhnout tak vyššího výkonu se serverem Nginx, jak je znázorněno v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="151af-292">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

```csharp
.ConfigureKestrel((context, options) =>
{
    options.ListenUnixSocket("/tmp/kestrel-test.sock");
    options.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testpassword");
    });
});
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="151af-293">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="151af-293">**Port 0**</span></span>

<span data-ttu-id="151af-294">Když číslo portu `0` určena Kestrel dynamicky váže dostupný port.</span><span class="sxs-lookup"><span data-stu-id="151af-294">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="151af-295">Následující příklad ukazuje, jak určit port, který Kestrel vázán skutečně za běhu:</span><span class="sxs-lookup"><span data-stu-id="151af-295">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="151af-296">Při spuštění aplikace Určuje výstup okna konzoly dynamický port, kde se dá kontaktovat aplikace:</span><span class="sxs-lookup"><span data-stu-id="151af-296">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

<span data-ttu-id="151af-297">**Konfigurační klíč a omezení proměnnou prostředí ASPNETCORE_URLS hostitele UseUrls argument příkazového řádku--adresy URL, adresy URL**</span><span class="sxs-lookup"><span data-stu-id="151af-297">**UseUrls, --urls command-line argument, urls host configuration key, and ASPNETCORE_URLS environment variable limitations**</span></span>

<span data-ttu-id="151af-298">Konfigurace koncových bodů pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="151af-298">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="151af-299">UseUrls</span><span class="sxs-lookup"><span data-stu-id="151af-299">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="151af-300">`--urls` argument příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="151af-300">`--urls` command-line argument</span></span>
* <span data-ttu-id="151af-301">`urls` Konfigurační klíč hostitele</span><span class="sxs-lookup"><span data-stu-id="151af-301">`urls` host configuration key</span></span>
* <span data-ttu-id="151af-302">`ASPNETCORE_URLS` Proměnná prostředí</span><span class="sxs-lookup"><span data-stu-id="151af-302">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="151af-303">Tyto metody jsou užitečné pro provádění kódu práce se servery než Kestrel.</span><span class="sxs-lookup"><span data-stu-id="151af-303">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="151af-304">Mějte ale na tato omezení:</span><span class="sxs-lookup"><span data-stu-id="151af-304">However, be aware of these limitations:</span></span>

* <span data-ttu-id="151af-305">Protokol SSL nelze použít s těchto přístupů, pokud výchozí certifikát je součástí konfigurace koncového bodu HTTPS (například pomocí `KestrelServerOptions` konfigurace nebo konfiguračního souboru, jak je znázorněno výše v tomto tématu).</span><span class="sxs-lookup"><span data-stu-id="151af-305">SSL can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="151af-306">Při i `Listen` a `UseUrls` přístupy se využívat současně, `Listen` přepsat koncových bodů `UseUrls` koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="151af-306">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="151af-307">**Konfigurace koncového bodu služby IIS**</span><span class="sxs-lookup"><span data-stu-id="151af-307">**IIS endpoint configuration**</span></span>

<span data-ttu-id="151af-308">Při použití služby IIS, adresa URL vazeb pro služby IIS změnit vazby jsou nastaveny buď `Listen` nebo `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="151af-308">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="151af-309">Další informace najdete v tématu [modul ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) tématu.</span><span class="sxs-lookup"><span data-stu-id="151af-309">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="151af-310">Ve výchozím nastavení, ASP.NET Core váže k `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="151af-310">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="151af-311">Konfigurace předpony adres URL a portů pro Kestrel pomocí:</span><span class="sxs-lookup"><span data-stu-id="151af-311">Configure URL prefixes and ports for Kestrel using:</span></span>

* <span data-ttu-id="151af-312">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) – metoda rozšíření</span><span class="sxs-lookup"><span data-stu-id="151af-312">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) extension method</span></span>
* <span data-ttu-id="151af-313">`--urls` argument příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="151af-313">`--urls` command-line argument</span></span>
* <span data-ttu-id="151af-314">`urls` Konfigurační klíč hostitele</span><span class="sxs-lookup"><span data-stu-id="151af-314">`urls` host configuration key</span></span>
* <span data-ttu-id="151af-315">ASP.NET Core systém konfigurace, včetně `ASPNETCORE_URLS` proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="151af-315">ASP.NET Core configuration system, including `ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="151af-316">Další informace o těchto metodách, naleznete v tématu [Hosting](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="151af-316">For more information on these methods, see [Hosting](xref:fundamentals/host/index).</span></span>

<span data-ttu-id="151af-317">**Konfigurace koncového bodu služby IIS**</span><span class="sxs-lookup"><span data-stu-id="151af-317">**IIS endpoint configuration**</span></span>

<span data-ttu-id="151af-318">Při použití služby IIS, adresa URL vazby pro službu IIS přepsat vazby nastavil `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="151af-318">When using IIS, the URL bindings for IIS override bindings set by `UseUrls`.</span></span> <span data-ttu-id="151af-319">Další informace najdete v tématu [modul ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) tématu.</span><span class="sxs-lookup"><span data-stu-id="151af-319">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a><span data-ttu-id="151af-320">Konfigurace přenosu</span><span class="sxs-lookup"><span data-stu-id="151af-320">Transport configuration</span></span>

<span data-ttu-id="151af-321">Verze technologie ASP.NET Core 2.1 Kestrel pro výchozí přenos je již podle Libuv ale místo toho podle spravované sokety.</span><span class="sxs-lookup"><span data-stu-id="151af-321">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="151af-322">Toto je zásadní změny pro aplikace ASP.NET Core 2.0 upgrade na verzi 2.1, které volají [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) a závisí na jednu z následujících balíčků:</span><span class="sxs-lookup"><span data-stu-id="151af-322">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) and depend on either of the following packages:</span></span>

* <span data-ttu-id="151af-323">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (přímý odkaz na balíček)</span><span class="sxs-lookup"><span data-stu-id="151af-323">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="151af-324">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="151af-324">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="151af-325">Pro ASP.NET Core 2.1 nebo novější projekty, které používají [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) a vyžadují použití Libuv:</span><span class="sxs-lookup"><span data-stu-id="151af-325">For ASP.NET Core 2.1 or later projects that use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and require the use of Libuv:</span></span>

* <span data-ttu-id="151af-326">Přidat závislost [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) balíček do souboru projektu vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="151af-326">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="2.1.0" />
    ```

* <span data-ttu-id="151af-327">Volání [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span><span class="sxs-lookup"><span data-stu-id="151af-327">Call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span></span>

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

::: moniker-end

### <a name="url-prefixes"></a><span data-ttu-id="151af-328">Předpony adres URL</span><span class="sxs-lookup"><span data-stu-id="151af-328">URL prefixes</span></span>

<span data-ttu-id="151af-329">Při použití `UseUrls`, `--urls` argument příkazového řádku, `urls` konfigurační klíč hostitele, nebo `ASPNETCORE_URLS` proměnné prostředí, předpony adres URL může být v některém z následujících formátů.</span><span class="sxs-lookup"><span data-stu-id="151af-329">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="151af-330">Platné jsou pouze předpony adres URL protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="151af-330">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="151af-331">Kestrel nepodporuje SSL při konfiguraci adresy URL vazby, které používají `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="151af-331">Kestrel doesn't support SSL when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="151af-332">Adresu IPv4 s číslem portu</span><span class="sxs-lookup"><span data-stu-id="151af-332">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="151af-333">`0.0.0.0` je zvláštní případ s vazbou na všechny adresy IPv4.</span><span class="sxs-lookup"><span data-stu-id="151af-333">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="151af-334">Adresa protokolu IPv6 s číslem portu</span><span class="sxs-lookup"><span data-stu-id="151af-334">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="151af-335">`[::]` je ekvivalentem IPv6 IPv4 `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="151af-335">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="151af-336">Název hostitele s číslem portu</span><span class="sxs-lookup"><span data-stu-id="151af-336">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="151af-337">Názvy hostitelů `*`, a `+`, nejsou speciální.</span><span class="sxs-lookup"><span data-stu-id="151af-337">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="151af-338">Nic není rozpoznán jako platná IP adresa nebo `localhost` vytvoří vazbu pro všechny IP adresy IPv6 a IPv4.</span><span class="sxs-lookup"><span data-stu-id="151af-338">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="151af-339">Pro vázání názvů jiného hostitele a různé aplikace ASP.NET Core na stejném portu, použijte [HTTP.sys](xref:fundamentals/servers/httpsys) nebo reverzní proxy server, jako je například Apache, IIS nebo Nginx.</span><span class="sxs-lookup"><span data-stu-id="151af-339">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="151af-340">Pokud nepoužíváte reverzního proxy serveru s hostitelem filtrování povolené, povolte [hostitele filtrování](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="151af-340">If not using a reverse proxy with host filtering enabled, enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="151af-341">Hostitel `localhost` název portu s číslem portu číslo nebo adresu zpětné smyčky IP</span><span class="sxs-lookup"><span data-stu-id="151af-341">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="151af-342">Když `localhost` určena Kestrel pokusí o připojení k rozhraní zpětné smyčky protokolu IPv4 a IPv6.</span><span class="sxs-lookup"><span data-stu-id="151af-342">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="151af-343">Pokud požadovaný port je používán jinou službou buď rozhraní zpětné smyčky, Kestrel nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="151af-343">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="151af-344">Pokud je buď rozhraní zpětné smyčky není k dispozici z jiného důvodu (většinu běžně, protože protokol IPv6 není podporován), Kestrel protokoluje upozornění.</span><span class="sxs-lookup"><span data-stu-id="151af-344">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="151af-345">Adresu IPv4 s číslem portu</span><span class="sxs-lookup"><span data-stu-id="151af-345">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="151af-346">`0.0.0.0` je zvláštní případ s vazbou na všechny adresy IPv4.</span><span class="sxs-lookup"><span data-stu-id="151af-346">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="151af-347">Adresa protokolu IPv6 s číslem portu</span><span class="sxs-lookup"><span data-stu-id="151af-347">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  <span data-ttu-id="151af-348">`[::]` je ekvivalentem IPv6 IPv4 `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="151af-348">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="151af-349">Název hostitele s číslem portu</span><span class="sxs-lookup"><span data-stu-id="151af-349">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="151af-350">Názvy hostitelů `*`, a `+` nejsou speciální.</span><span class="sxs-lookup"><span data-stu-id="151af-350">Host names, `*`, and `+` aren't special.</span></span> <span data-ttu-id="151af-351">Cokoli, co není rozpoznaný IP adresu nebo `localhost` vytvoří vazbu pro všechny IP adresy IPv6 a IPv4.</span><span class="sxs-lookup"><span data-stu-id="151af-351">Anything that isn't a recognized IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="151af-352">Pro vázání názvů jiného hostitele a různé aplikace ASP.NET Core na stejném portu, použijte [WebListener](xref:fundamentals/servers/weblistener) nebo reverzní proxy server, jako je například Apache, IIS nebo Nginx.</span><span class="sxs-lookup"><span data-stu-id="151af-352">To bind different host names to different ASP.NET Core apps on the same port, use [WebListener](xref:fundamentals/servers/weblistener) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="151af-353">Hostitel `localhost` název portu s číslem portu číslo nebo adresu zpětné smyčky IP</span><span class="sxs-lookup"><span data-stu-id="151af-353">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="151af-354">Když `localhost` určena Kestrel pokusí o připojení k rozhraní zpětné smyčky protokolu IPv4 a IPv6.</span><span class="sxs-lookup"><span data-stu-id="151af-354">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="151af-355">Pokud požadovaný port je používán jinou službou buď rozhraní zpětné smyčky, Kestrel nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="151af-355">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="151af-356">Pokud je buď rozhraní zpětné smyčky není k dispozici z jiného důvodu (většinu běžně, protože protokol IPv6 není podporován), Kestrel protokoluje upozornění.</span><span class="sxs-lookup"><span data-stu-id="151af-356">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

* <span data-ttu-id="151af-357">Unixovému soketu</span><span class="sxs-lookup"><span data-stu-id="151af-357">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock
  ```

<span data-ttu-id="151af-358">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="151af-358">**Port 0**</span></span>

<span data-ttu-id="151af-359">Pokud je číslo portu `0` určena Kestrel dynamicky váže dostupný port.</span><span class="sxs-lookup"><span data-stu-id="151af-359">When the port number is `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="151af-360">Vytvoření vazby na port `0` povoleny pro název hostitele nebo IP adresy s výjimkou pro `localhost`.</span><span class="sxs-lookup"><span data-stu-id="151af-360">Binding to port `0` is allowed for any host name or IP except for `localhost`.</span></span>

<span data-ttu-id="151af-361">Při spuštění aplikace Určuje výstup okna konzoly dynamický port, kde se dá kontaktovat aplikace:</span><span class="sxs-lookup"><span data-stu-id="151af-361">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="151af-362">**Předpony adres URL pro protokol SSL**</span><span class="sxs-lookup"><span data-stu-id="151af-362">**URL prefixes for SSL**</span></span>

<span data-ttu-id="151af-363">Pokud volání `UseHttps` rozšiřující metoda ji nezapomeňte zahrnout předpony adres URL s `https:`:</span><span class="sxs-lookup"><span data-stu-id="151af-363">If calling the `UseHttps` extension method, be sure to include URL prefixes with `https:`:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel(options =>
    {
        options.UseHttps("testCert.pfx", "testPassword");
    })
   .UseUrls("http://localhost:5000", "https://localhost:5001")
   .UseContentRoot(Directory.GetCurrentDirectory())
   .UseStartup<Startup>()
   .Build();
```

> [!NOTE]
> <span data-ttu-id="151af-364">Na stejném portu nemůže být hostovaná protokolů HTTPS a HTTP.</span><span class="sxs-lookup"><span data-stu-id="151af-364">HTTPS and HTTP can't be hosted on the same port.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

::: moniker-end

## <a name="host-filtering"></a><span data-ttu-id="151af-365">Hostitel filtrování</span><span class="sxs-lookup"><span data-stu-id="151af-365">Host filtering</span></span>

<span data-ttu-id="151af-366">I když Kestrel podporuje konfigurace, například podle předpon `http://example.com:5000`, Kestrel do značné míry ignoruje název hostitele.</span><span class="sxs-lookup"><span data-stu-id="151af-366">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="151af-367">Hostitel `localhost` je zvláštní případ použité pro vazbu na adresu zpětné smyčky adresy.</span><span class="sxs-lookup"><span data-stu-id="151af-367">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="151af-368">Všechny hostitele, jiné než explicitních IP adresu vytvoří vazbu na všechny veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="151af-368">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="151af-369">Žádná z těchto informací slouží k ověření požadavku `Host` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="151af-369">None of this information is used to validate request `Host` headers.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="151af-370">Alternativním řešením je hostitelem za reverzní proxy server s filtrováním hlavičky hostitele.</span><span class="sxs-lookup"><span data-stu-id="151af-370">As a workaround, host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="151af-371">Toto je jediný podporovaný scénář pro Kestrel v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="151af-371">This is the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="151af-372">Jako alternativní řešení použít k filtrování požadavků podle middlewaru `Host` hlavičky:</span><span class="sxs-lookup"><span data-stu-id="151af-372">As a workaround, use middleware to filter requests by the `Host` header:</span></span>

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

<span data-ttu-id="151af-373">Zaregistrujte předchozí `HostFilteringMiddleware` v `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="151af-373">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="151af-374">Všimněte si, že [řazení zápisu middleware](xref:fundamentals/middleware/index#order) je důležité.</span><span class="sxs-lookup"><span data-stu-id="151af-374">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#order) is important.</span></span> <span data-ttu-id="151af-375">Registrace se budou objevovat hned po registraci diagnostických Middleware (například `app.UseExceptionHandler`).</span><span class="sxs-lookup"><span data-stu-id="151af-375">Registration should occur immediately after Diagnostic Middleware registration (for example, `app.UseExceptionHandler`).</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

<span data-ttu-id="151af-376">Middleware očekává, že `AllowedHosts` klíče v *appsettings.json*/*appsettings.\< EnvironmentName > .json*.</span><span class="sxs-lookup"><span data-stu-id="151af-376">The middleware expects an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="151af-377">Hodnota je středníkem oddělený seznam názvů hostitele bez čísla portů:</span><span class="sxs-lookup"><span data-stu-id="151af-377">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="151af-378">Jako alternativní řešení použijte hostitele filtrování middlewaru.</span><span class="sxs-lookup"><span data-stu-id="151af-378">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="151af-379">Poskytuje Middleware filtrování hostitele [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) balíček, který je součástí [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="151af-379">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="151af-380">Přidá middleware [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), který volá [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span><span class="sxs-lookup"><span data-stu-id="151af-380">The middleware is added by [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="151af-381">Middleware filtrování hostitele je ve výchozím nastavení zakázána.</span><span class="sxs-lookup"><span data-stu-id="151af-381">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="151af-382">Chcete-li povolit middleware, definujte `AllowedHosts` klíče v *appsettings.json*/*appsettings.\< EnvironmentName > .json*.</span><span class="sxs-lookup"><span data-stu-id="151af-382">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="151af-383">Hodnota je středníkem oddělený seznam názvů hostitele bez čísla portů:</span><span class="sxs-lookup"><span data-stu-id="151af-383">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="151af-384">*appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="151af-384">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="151af-385">[Předané záhlaví Middleware](xref:host-and-deploy/proxy-load-balancer) má také [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) možnost.</span><span class="sxs-lookup"><span data-stu-id="151af-385">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) option.</span></span> <span data-ttu-id="151af-386">Přesměrovaná záhlaví Middleware a filtrování Middleware hostitele mají podobné funkce pro různé scénáře.</span><span class="sxs-lookup"><span data-stu-id="151af-386">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="151af-387">Nastavení `AllowedHosts` předané Middleware hlavičky je vhodné při hlavičku hostitele nezachová při předávání žádostí s reverzní proxy server nebo nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="151af-387">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the Host header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="151af-388">Nastavení `AllowedHosts` s Middlewarem filtrování hostitele je vhodné při Kestrel slouží jako edge server nebo když je hlavička hostitele přímo předán.</span><span class="sxs-lookup"><span data-stu-id="151af-388">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as an edge server or when the Host header is directly forwarded.</span></span>
>
> <span data-ttu-id="151af-389">Další informace o předávaných Middleware záhlaví, naleznete v tématu [konfigurace ASP.NET Core práci se servery proxy a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="151af-389">For more information on Forwarded Headers Middleware, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="151af-390">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="151af-390">Additional resources</span></span>

* [<span data-ttu-id="151af-391">Vynucení protokolu HTTPS</span><span class="sxs-lookup"><span data-stu-id="151af-391">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="151af-392">Kestrel zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="151af-392">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
* [<span data-ttu-id="151af-393">RFC 7230: Syntaxe a směrování zpráv (oddíl 5.4: hostitele)</span><span class="sxs-lookup"><span data-stu-id="151af-393">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
* [<span data-ttu-id="151af-394">Konfigurace ASP.NET Core práci se servery proxy a nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="151af-394">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
