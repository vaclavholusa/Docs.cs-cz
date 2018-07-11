---
title: Implementace serveru webové kestrel v ASP.NET Core
author: rick-anderson
description: Další informace o Kestrel, napříč platformami webový server pro ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/02/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 62649351271deebcf1ed9d2f8b2258bed3478989
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126940"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="52d0c-103">Implementace serveru webové kestrel v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="52d0c-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="52d0c-104">Podle [Petr Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), a [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="52d0c-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="52d0c-105">Kestrel je platformově univerzální [webového serveru pro ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="52d0c-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="52d0c-106">Kestrel je webový server, který je obsažen ve výchozím nastavení v šablonách projektů ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="52d0c-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="52d0c-107">Kestrel podporuje následující funkce:</span><span class="sxs-lookup"><span data-stu-id="52d0c-107">Kestrel supports the following features:</span></span>

* <span data-ttu-id="52d0c-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="52d0c-108">HTTPS</span></span>
* <span data-ttu-id="52d0c-109">Neprůhledný upgrade nepoužívá k zapnutí [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="52d0c-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="52d0c-110">Soketů systému UNIX pro vysoký výkon za serveru Nginx</span><span class="sxs-lookup"><span data-stu-id="52d0c-110">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="52d0c-111">Kestrel se podporuje na všech platformách a verze, které podporuje .NET Core.</span><span class="sxs-lookup"><span data-stu-id="52d0c-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="52d0c-112">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="52d0c-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="52d0c-113">Kdy použít Kestrel pomocí reverzního proxy serveru</span><span class="sxs-lookup"><span data-stu-id="52d0c-113">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="52d0c-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="52d0c-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="52d0c-115">Kestrel můžete použít samostatně nebo se *reverzní proxy server*, jako je například Apache, IIS nebo Nginx.</span><span class="sxs-lookup"><span data-stu-id="52d0c-115">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="52d0c-116">Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je na Kestrel po některé předběžného zpracování.</span><span class="sxs-lookup"><span data-stu-id="52d0c-116">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel komunikuje přímo s Internetu bez reverzní proxy server](kestrel/_static/kestrel-to-internet2.png)

![Kestrel nepřímo komunikuje přes Internet prostřednictvím reverzního proxy serveru, jako je například Apache, IIS nebo Nginx](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="52d0c-119">Buď konfiguraci&mdash;s nebo bez něj reverzní proxy server&mdash;je platný a podporované konfigurace pro hostování pro ASP.NET Core 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="52d0c-119">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="52d0c-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="52d0c-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="52d0c-121">Pokud aplikace přijímá požadavky jenom z interní sítě, Kestrel lze použít přímo jako server aplikace.</span><span class="sxs-lookup"><span data-stu-id="52d0c-121">If an app accepts requests only from an internal network, Kestrel can be used directly as the app's server.</span></span>

![Kestrel komunikuje přímo s vaší interní sítě](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="52d0c-123">Pokud je zveřejnit aplikaci k Internetu, použít službu IIS, serveru Nginx nebo Apache jako *reverzní proxy server*.</span><span class="sxs-lookup"><span data-stu-id="52d0c-123">If you expose your app to the Internet, use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="52d0c-124">Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je na Kestrel po některé předběžného zpracování.</span><span class="sxs-lookup"><span data-stu-id="52d0c-124">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel nepřímo komunikuje přes Internet prostřednictvím reverzního proxy serveru, jako je například Apache, IIS nebo Nginx](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="52d0c-126">Reverzní proxy je vyžadován pro nasazení hraniční (vystavené pro provoz z Internetu) z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="52d0c-126">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="52d0c-127">Verze 1.x Kestrel nemají úplný doplněk obranu před útoky, jako je například omezení počtu souběžných připojení, odpovídající časové limity a omezení velikosti.</span><span class="sxs-lookup"><span data-stu-id="52d0c-127">The 1.x versions of Kestrel don't have a full complement of defenses against attacks, such as appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="52d0c-128">Scénář reverzního proxy serveru existuje, pokud existuje víc aplikací, které sdílejí stejné IP adresy a portu, které běží na jednom serveru.</span><span class="sxs-lookup"><span data-stu-id="52d0c-128">A reverse proxy scenario exists when there are multiple apps that share the same IP and port running on a single server.</span></span> <span data-ttu-id="52d0c-129">Kestrel tento scénář nepodporuje, protože Kestrel nepodporuje sdílení stejné IP adresy a portu mezi více procesy.</span><span class="sxs-lookup"><span data-stu-id="52d0c-129">Kestrel doesn't support this scenario because Kestrel doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="52d0c-130">Když Kestrel je nakonfigurovaná k naslouchání na portu, zpracovává Kestrel veškerý síťový provoz na tento port bez ohledu na to, požadavky se hlavička hostitele.</span><span class="sxs-lookup"><span data-stu-id="52d0c-130">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' host header.</span></span> <span data-ttu-id="52d0c-131">Reverzní proxy server, který můžete sdílet porty má schopnost Kestrel na jedinečné IP adresy a portu směrování žádostí.</span><span class="sxs-lookup"><span data-stu-id="52d0c-131">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="52d0c-132">I v případě reverzního proxy serveru není povinné, pomocí reverzního proxy serveru může být dobrou volbou:</span><span class="sxs-lookup"><span data-stu-id="52d0c-132">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice:</span></span>

* <span data-ttu-id="52d0c-133">Může být omezena vystavené veřejné útoku na aplikace, které hostuje.</span><span class="sxs-lookup"><span data-stu-id="52d0c-133">It can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="52d0c-134">Poskytuje další úroveň ochrany a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="52d0c-134">It provides an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="52d0c-135">Integrace může lépe se stávající infrastrukturou.</span><span class="sxs-lookup"><span data-stu-id="52d0c-135">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="52d0c-136">Zjednodušuje Vyrovnávání zatížení a konfigurace protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="52d0c-136">It simplifies load balancing and SSL configuration.</span></span> <span data-ttu-id="52d0c-137">Reverzní proxy server vyžaduje certifikát SSL, a tento server může komunikovat s aplikačních serverů v interní síti přes standardní HTTP.</span><span class="sxs-lookup"><span data-stu-id="52d0c-137">Only the reverse proxy server requires an SSL certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="52d0c-138">Pokud nepoužíváte reverzního proxy serveru s hostitelem filtrování povolena, [hostitele filtrování](#host-filtering) musí být povolené.</span><span class="sxs-lookup"><span data-stu-id="52d0c-138">If not using a reverse proxy with host filtering enabled, [host filtering](#host-filtering) must be enabled.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="52d0c-139">Jak používat Kestrel v aplikacích ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="52d0c-139">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="52d0c-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="52d0c-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="52d0c-141">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) balíčku je součástí [Microsoft.AspNetCore.App Microsoft.aspnetcore.all] (xref:fundamentals / Microsoft.aspnetcore.all app) (ASP.NET Core 2.1 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="52d0c-141">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage] (xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="52d0c-142">Šablony projektů ASP.NET Core pomocí Kestrel ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="52d0c-142">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="52d0c-143">V *Program.cs*, kód volání šablony [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), který volá [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) na pozadí.</span><span class="sxs-lookup"><span data-stu-id="52d0c-143">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="52d0c-144">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="52d0c-144">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="52d0c-145">Nainstalujte [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="52d0c-145">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="52d0c-146">Volání [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) rozšiřující metody na [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) v `Main` metodu, zadáte některý [Kestrel možnosti](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) vyžaduje, jak je znázorněno v následující části.</span><span class="sxs-lookup"><span data-stu-id="52d0c-146">Call the [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) in the `Main` method, specifying any [Kestrel options](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) required, as shown in the next section.</span></span>

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="52d0c-147">Možnosti kestrel</span><span class="sxs-lookup"><span data-stu-id="52d0c-147">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="52d0c-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="52d0c-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="52d0c-149">Webový server Kestrel má omezení možnosti konfigurace, které jsou obzvláště užitečné v nasazeních s přístupem k Internetu.</span><span class="sxs-lookup"><span data-stu-id="52d0c-149">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="52d0c-150">Pár důležitých omezení, které se dají přizpůsobit:</span><span class="sxs-lookup"><span data-stu-id="52d0c-150">A few important limits that can be customized:</span></span>

* <span data-ttu-id="52d0c-151">Maximální počet klientských připojení</span><span class="sxs-lookup"><span data-stu-id="52d0c-151">Maximum client connections</span></span>
* <span data-ttu-id="52d0c-152">Velikost textu maximální požadavku</span><span class="sxs-lookup"><span data-stu-id="52d0c-152">Maximum request body size</span></span>
* <span data-ttu-id="52d0c-153">Minimální požadavek tělo přenosová rychlost</span><span class="sxs-lookup"><span data-stu-id="52d0c-153">Minimum request body data rate</span></span>

<span data-ttu-id="52d0c-154">Nastavte na tyto a další omezení [omezení](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) vlastnost [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) třídy.</span><span class="sxs-lookup"><span data-stu-id="52d0c-154">Set these and other constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="52d0c-155">`Limits` Vlastnost obsahuje instanci [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) třídy.</span><span class="sxs-lookup"><span data-stu-id="52d0c-155">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

<span data-ttu-id="52d0c-156">**Maximální počet klientských připojení**</span><span class="sxs-lookup"><span data-stu-id="52d0c-156">**Maximum client connections**</span></span>

[<span data-ttu-id="52d0c-157">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="52d0c-157">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="52d0c-158">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="52d0c-158">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

<span data-ttu-id="52d0c-159">Lze nastavit maximální počet souběžných otevřená připojení TCP pro celou aplikaci s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="52d0c-159">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="52d0c-160">Neexistuje samostatné limit pro připojení, která se upgradovaly z protokolu HTTP nebo HTTPS na jiné protokol (například v požadavku Websocket).</span><span class="sxs-lookup"><span data-stu-id="52d0c-160">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="52d0c-161">Po dokončení upgradu spojení se započítává `MaxConcurrentConnections` limit.</span><span class="sxs-lookup"><span data-stu-id="52d0c-161">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="52d0c-162">Maximální počet připojení je neomezený počet (null) ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="52d0c-162">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="52d0c-163">**Velikost textu maximální požadavku**</span><span class="sxs-lookup"><span data-stu-id="52d0c-163">**Maximum request body size**</span></span>

[<span data-ttu-id="52d0c-164">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="52d0c-164">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="52d0c-165">Výchozí velikost těla maximální požadavku je 30,000,000 bajtů, což je přibližně 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="52d0c-165">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="52d0c-166">Doporučený postup, chcete-li přepsat omezení v aplikaci ASP.NET Core MVC je použít [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) atributu na metodu akce:</span><span class="sxs-lookup"><span data-stu-id="52d0c-166">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="52d0c-167">Tady je příklad, který ukazuje, jak nakonfigurovat omezení pro aplikace u každého požadavku:</span><span class="sxs-lookup"><span data-stu-id="52d0c-167">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="52d0c-168">Můžete přepsat nastavení pro konkrétní žádost a v middlewaru:</span><span class="sxs-lookup"><span data-stu-id="52d0c-168">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="52d0c-169">Pokud se pokusíte nakonfigurovat limit na vyžádání po spuštění aplikace k přečtení požadavku, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="52d0c-169">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="52d0c-170">Je `IsReadOnly` vlastnost, která označuje, zda `MaxRequestBodySize` vlastnost je ve stavu jen pro čtení, což znamená, je příliš pozdě Konfigurace limitu.</span><span class="sxs-lookup"><span data-stu-id="52d0c-170">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="52d0c-171">**Minimální požadavek tělo přenosová rychlost**</span><span class="sxs-lookup"><span data-stu-id="52d0c-171">**Minimum request body data rate**</span></span>

[<span data-ttu-id="52d0c-172">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="52d0c-172">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="52d0c-173">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="52d0c-173">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="52d0c-174">Kestrel kontroluje každou sekundu, pokud je dat přicházejících u určenou míru v bajtech/sekundu.</span><span class="sxs-lookup"><span data-stu-id="52d0c-174">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="52d0c-175">Pokud míra klesne pod minimální, vypršení časového limitu připojení. Období odkladu je množství času, aby Kestrel poskytuje klienta ke zvýšení jeho míra odesílání z až Toto minimum, frekvence není kontrolován během této doby.</span><span class="sxs-lookup"><span data-stu-id="52d0c-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="52d0c-176">Období odkladu pomáhá předejít, vyřadit připojení, která původně odesílají data s nízkou rychlostí kvůli zpomalit TCP-start.</span><span class="sxs-lookup"><span data-stu-id="52d0c-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="52d0c-177">Výchozí minimální rychlost je 240 bajtů za sekundu s období odkladu 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="52d0c-177">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="52d0c-178">Minimální sazba platí také pro odpověď.</span><span class="sxs-lookup"><span data-stu-id="52d0c-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="52d0c-179">Kód pro nastavení limitu požadavků a omezení odpovědi je stejná s výjimkou s `RequestBody` nebo `Response` v názvech vlastností a interface.</span><span class="sxs-lookup"><span data-stu-id="52d0c-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="52d0c-180">Tady je příklad, který ukazuje, jak nakonfigurovat minimální datové sazby v *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="52d0c-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-7)]

<span data-ttu-id="52d0c-181">Sazby za žádost můžete nakonfigurovat v middlewaru:</span><span class="sxs-lookup"><span data-stu-id="52d0c-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="52d0c-182">Informace o dalších možnostech Kestrel a omezení najdete tady:</span><span class="sxs-lookup"><span data-stu-id="52d0c-182">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="52d0c-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="52d0c-183">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="52d0c-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="52d0c-184">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="52d0c-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="52d0c-185">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="52d0c-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="52d0c-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="52d0c-187">Informace o možnostech Kestrel a omezení najdete tady:</span><span class="sxs-lookup"><span data-stu-id="52d0c-187">For information about Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="52d0c-188">Třída KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="52d0c-188">KestrelServerOptions class</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [<span data-ttu-id="52d0c-189">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="52d0c-189">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

---

### <a name="endpoint-configuration"></a><span data-ttu-id="52d0c-190">Konfigurace koncového bodu</span><span class="sxs-lookup"><span data-stu-id="52d0c-190">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="52d0c-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="52d0c-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="52d0c-192">Ve výchozím nastavení, ASP.NET Core váže k `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="52d0c-192">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="52d0c-193">Volání [naslouchání](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) nebo [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) metody [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) konfigurace předpony adres URL a portů pro Kestrel.</span><span class="sxs-lookup"><span data-stu-id="52d0c-193">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span> <span data-ttu-id="52d0c-194">`UseUrls`, `--urls` argument příkazového řádku, `urls` konfigurační klíč hostitele a `ASPNETCORE_URLS` proměnnou prostředí také pracovní ale mají omezení, později uvedené v této části.</span><span class="sxs-lookup"><span data-stu-id="52d0c-194">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section.</span></span>

<span data-ttu-id="52d0c-195">`urls` Konfigurační klíč hostitele musí pocházet z konfigurace hostitele, není konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="52d0c-195">The `urls` host configuration key must come from the host configuration, not the app configuration.</span></span> <span data-ttu-id="52d0c-196">Přidávání `urls` klíče a hodnoty *appsettings.json* konfigurace hostitele nemá vliv, protože hostitel je zcela inicializován době konfigurace je pro čtení, z konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="52d0c-196">Adding a `urls` key and value to *appsettings.json* doesn't affect host configuration because the host is completely initialized by the time the configuration is read from the configuration file.</span></span> <span data-ttu-id="52d0c-197">Ale `urls` klíče v *appsettings.json* jde použít s [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) na tvůrce hostitele a nakonfigurujte hostitele:</span><span class="sxs-lookup"><span data-stu-id="52d0c-197">However, a `urls` key in *appsettings.json* can be used with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) on the host builder to configure the host:</span></span>

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

<span data-ttu-id="52d0c-198">Ve výchozím nastavení ASP.NET Core váže na:</span><span class="sxs-lookup"><span data-stu-id="52d0c-198">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="52d0c-199">`https://localhost:5001` (Pokud je místní vývojový certifikát k dispozici)</span><span class="sxs-lookup"><span data-stu-id="52d0c-199">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="52d0c-200">Certifikát pro vývoj se vytvoří:</span><span class="sxs-lookup"><span data-stu-id="52d0c-200">A development certificate is created:</span></span>

* <span data-ttu-id="52d0c-201">Když [.NET Core SDK](/dotnet/core/sdk) je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="52d0c-201">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="52d0c-202">[Nástroji dev-certs](xref:aspnetcore-2.1#https) slouží k vytvoření certifikátu.</span><span class="sxs-lookup"><span data-stu-id="52d0c-202">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="52d0c-203">Některé prohlížeče vyžadují, abyste udělili explicitní oprávnění pro prohlížeč důvěřovat certifikátům místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="52d0c-203">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="52d0c-204">ASP.NET Core 2.1 a vyšší šablony projektů konfigurace aplikací ve výchozím nastavení spouští na protokol HTTPS a zahrnout [přesměrování protokolu HTTPS a HSTS podporují](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="52d0c-204">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="52d0c-205">Volání [naslouchání](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) nebo [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) metody [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) konfigurace předpony adres URL a portů pro Kestrel.</span><span class="sxs-lookup"><span data-stu-id="52d0c-205">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="52d0c-206">`UseUrls`, `--urls` argument příkazového řádku, `urls` konfigurační klíč hostitele a `ASPNETCORE_URLS` proměnnou prostředí také pracovní ale mají omezení, později uvedené v této části (výchozí certifikát musí být k dispozici pro koncový bod HTTPS Konfigurace).</span><span class="sxs-lookup"><span data-stu-id="52d0c-206">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="52d0c-207">ASP.NET Core 2.1 `KestrelServerOptions` konfigurace:</span><span class="sxs-lookup"><span data-stu-id="52d0c-207">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

<span data-ttu-id="52d0c-208">**ConfigureEndpointDefaults (akce&lt;ListenOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="52d0c-208">**ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)**</span></span>  
<span data-ttu-id="52d0c-209">Určuje konfiguraci `Action` pro spuštění v každý zadaný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="52d0c-209">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="52d0c-210">Volání `ConfigureEndpointDefaults` více než jednou nahradí předchozí `Action`s poslední `Action` zadané.</span><span class="sxs-lookup"><span data-stu-id="52d0c-210">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="52d0c-211">**ConfigureHttpsDefaults (akce&lt;HttpsConnectionAdapterOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="52d0c-211">**ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)**</span></span>  
<span data-ttu-id="52d0c-212">Určuje konfiguraci `Action` ke spuštění pro každý koncový bod HTTPS.</span><span class="sxs-lookup"><span data-stu-id="52d0c-212">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="52d0c-213">Volání `ConfigureHttpsDefaults` více než jednou nahradí předchozí `Action`s poslední `Action` zadané.</span><span class="sxs-lookup"><span data-stu-id="52d0c-213">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="52d0c-214">**Configure(IConfiguration)**</span><span class="sxs-lookup"><span data-stu-id="52d0c-214">**Configure(IConfiguration)**</span></span>  
<span data-ttu-id="52d0c-215">Vytvoří zavaděč konfigurace pro nastavení Kestrel, který přebírá [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) jako vstup.</span><span class="sxs-lookup"><span data-stu-id="52d0c-215">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="52d0c-216">Konfigurace musí být určená ke konfiguračnímu oddílu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="52d0c-216">The configuration must be scoped to the configuration section for Kestrel.</span></span>

<span data-ttu-id="52d0c-217">**ListenOptions.UseHttps**</span><span class="sxs-lookup"><span data-stu-id="52d0c-217">**ListenOptions.UseHttps**</span></span>  
<span data-ttu-id="52d0c-218">Nakonfigurujte Kestrel k používání HTTPS.</span><span class="sxs-lookup"><span data-stu-id="52d0c-218">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="52d0c-219">`ListenOptions.UseHttps` rozšíření:</span><span class="sxs-lookup"><span data-stu-id="52d0c-219">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="52d0c-220">`UseHttps` &ndash; Nakonfigurujte Kestrel k používání HTTPS pomocí certifikátu výchozí.</span><span class="sxs-lookup"><span data-stu-id="52d0c-220">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="52d0c-221">Vyvolá výjimku, pokud je nakonfigurovaný žádný výchozí certifikát.</span><span class="sxs-lookup"><span data-stu-id="52d0c-221">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="52d0c-222">`ListenOptions.UseHttps` Parametry:</span><span class="sxs-lookup"><span data-stu-id="52d0c-222">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="52d0c-223">`filename` je název a cesta k souboru soubor certifikátu, relativní k adresáři, který obsahuje soubory obsahu aplikace.</span><span class="sxs-lookup"><span data-stu-id="52d0c-223">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="52d0c-224">`password` je heslo pro přístup k datům certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="52d0c-224">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="52d0c-225">`configureOptions` je `Action` ke konfiguraci `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="52d0c-225">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="52d0c-226">Vrátí `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="52d0c-226">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="52d0c-227">`storeName` je do úložiště certifikátů, ze kterého se má načíst certifikát.</span><span class="sxs-lookup"><span data-stu-id="52d0c-227">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="52d0c-228">`subject` je název subjektu certifikátu.</span><span class="sxs-lookup"><span data-stu-id="52d0c-228">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="52d0c-229">`allowInvalid` Označuje, pokud neplatné certifikáty by měl být, jako jsou certifikáty podepsané svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="52d0c-229">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="52d0c-230">`location` je umístění úložiště pro načtení certifikátu.</span><span class="sxs-lookup"><span data-stu-id="52d0c-230">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="52d0c-231">`serverCertificate` je certifikát X.509.</span><span class="sxs-lookup"><span data-stu-id="52d0c-231">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="52d0c-232">V produkčním prostředí musí být explicitně nakonfigurován protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="52d0c-232">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="52d0c-233">Minimálně je třeba zadat výchozího certifikátu.</span><span class="sxs-lookup"><span data-stu-id="52d0c-233">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="52d0c-234">Podporované konfigurace je popsáno dále:</span><span class="sxs-lookup"><span data-stu-id="52d0c-234">Supported configurations described next:</span></span>

* <span data-ttu-id="52d0c-235">Žádná konfigurace</span><span class="sxs-lookup"><span data-stu-id="52d0c-235">No configuration</span></span>
* <span data-ttu-id="52d0c-236">Nahraďte výchozí certifikát z konfigurace</span><span class="sxs-lookup"><span data-stu-id="52d0c-236">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="52d0c-237">Změnit výchozí nastavení v kódu</span><span class="sxs-lookup"><span data-stu-id="52d0c-237">Change the defaults in code</span></span>

<span data-ttu-id="52d0c-238">*Žádná konfigurace*</span><span class="sxs-lookup"><span data-stu-id="52d0c-238">*No configuration*</span></span>

<span data-ttu-id="52d0c-239">Naslouchá kestrel `http://localhost:5000` a `https://localhost:5001` (Pokud je k dispozici výchozí cert).</span><span class="sxs-lookup"><span data-stu-id="52d0c-239">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="52d0c-240">Určení adres URL pomocí:</span><span class="sxs-lookup"><span data-stu-id="52d0c-240">Specify URLs using the:</span></span>

* <span data-ttu-id="52d0c-241">`ASPNETCORE_URLS` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="52d0c-241">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="52d0c-242">`--urls` argument příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="52d0c-242">`--urls` command-line argument.</span></span>
* <span data-ttu-id="52d0c-243">`urls` Konfigurační klíč hostitele.</span><span class="sxs-lookup"><span data-stu-id="52d0c-243">`urls` host configuration key.</span></span>
* <span data-ttu-id="52d0c-244">`UseUrls` metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="52d0c-244">`UseUrls` extension method.</span></span>

<span data-ttu-id="52d0c-245">Další informace najdete v tématu [adresy URL serveru](xref:fundamentals/host/web-host#server-urls) a [konfigurace přepisování](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="52d0c-245">For more information, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="52d0c-246">Hodnota zadaná pomocí těchto přístupů může být jeden nebo více HTTP a HTTPS koncové body (HTTPS Pokud je k dispozici výchozí cert).</span><span class="sxs-lookup"><span data-stu-id="52d0c-246">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="52d0c-247">Nakonfigurujte tuto hodnotu jako seznam oddělený středníkem (například `"Urls": "http://localhost:8000;http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="52d0c-247">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="52d0c-248">*Nahraďte výchozí certifikát z konfigurace*</span><span class="sxs-lookup"><span data-stu-id="52d0c-248">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="52d0c-249">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) volání `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` ve výchozím nastavení se načíst konfiguraci Kestrel.</span><span class="sxs-lookup"><span data-stu-id="52d0c-249">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="52d0c-250">Schéma konfigurace k nastavení výchozí HTTPS aplikace je k dispozici pro Kestrel.</span><span class="sxs-lookup"><span data-stu-id="52d0c-250">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="52d0c-251">Konfigurovat několik koncových bodů, včetně adresy URL a certifikáty, které chcete použít, ze souboru na disku nebo z úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="52d0c-251">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="52d0c-252">V následujícím *appsettings.json* příkladu:</span><span class="sxs-lookup"><span data-stu-id="52d0c-252">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="52d0c-253">Nastavte **AllowInvalid** k `true` tak, aby povolovala použití neplatné certifikáty (například certifikáty podepsané svým držitelem).</span><span class="sxs-lookup"><span data-stu-id="52d0c-253">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="52d0c-254">Libovolný koncový bod HTTPS, který nemá určenou certifikát (**HttpsDefaultCert** v následujícím příkladu) spadne zpět na cert definované v části **certifikáty** > **výchozí**  nebo certifikát pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="52d0c-254">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="52d0c-255">O alternativu k použití **cesta** a **heslo** pro jakýkoliv certifikát je uzel a určete certifikát, pomocí polí úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="52d0c-255">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="52d0c-256">Například **certifikáty** > **výchozí** certifikát lze zadat jako:</span><span class="sxs-lookup"><span data-stu-id="52d0c-256">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="52d0c-257">Schéma poznámek:</span><span class="sxs-lookup"><span data-stu-id="52d0c-257">Schema notes:</span></span>

* <span data-ttu-id="52d0c-258">Koncové body názvy jsou malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="52d0c-258">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="52d0c-259">Například `HTTPS` a `Https` jsou platné.</span><span class="sxs-lookup"><span data-stu-id="52d0c-259">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="52d0c-260">`Url` Parametr je povinný pro každý koncový bod.</span><span class="sxs-lookup"><span data-stu-id="52d0c-260">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="52d0c-261">Formát pro tento parametr je stejné jako na nejvyšší úrovni `Urls` parametr konfigurace s výjimkou, že je omezena na jednu hodnotu.</span><span class="sxs-lookup"><span data-stu-id="52d0c-261">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="52d0c-262">Nahraďte tyto koncové body jsou definované na nejvyšší úrovni `Urls` místo přidávání k nim.</span><span class="sxs-lookup"><span data-stu-id="52d0c-262">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="52d0c-263">Koncové body definované v kódu prostřednictvím `Listen` jsou kumulativní se koncové body definované v konfiguračním oddílu.</span><span class="sxs-lookup"><span data-stu-id="52d0c-263">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="52d0c-264">`Certificate` Část je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="52d0c-264">The `Certificate` section is optional.</span></span> <span data-ttu-id="52d0c-265">Pokud `Certificate` oddílu není zadán, budou použity výchozí hodnoty definované v předchozích případech.</span><span class="sxs-lookup"><span data-stu-id="52d0c-265">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="52d0c-266">Pokud nejsou k dispozici žádné výchozí hodnoty, vyvolá výjimku, server a nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="52d0c-266">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="52d0c-267">`Certificate` Části podporuje obě **cesta**&ndash;**heslo** a **subjektu**&ndash;**Store** certifikáty.</span><span class="sxs-lookup"><span data-stu-id="52d0c-267">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="52d0c-268">Tímto způsobem lze definovat libovolný počet koncových bodů tak dlouho, dokud není způsobují konflikty portu.</span><span class="sxs-lookup"><span data-stu-id="52d0c-268">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="52d0c-269">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` Vrátí `KestrelConfigurationLoader` s `.Endpoint(string name, options => { })` metodu, která slouží k doplnění nastavení konfigurovaný koncový bod:</span><span class="sxs-lookup"><span data-stu-id="52d0c-269">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="52d0c-270">Můžete také přímý přístup k `KestrelServerOptions.ConfigurationLoader` chcete zachovat iterace na existující zavaděč, jako je ta poskytuje [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="52d0c-270">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

* <span data-ttu-id="52d0c-271">Oddíl konfigurace pro každý koncový bod je k dispozici na možnosti v `Endpoint` metodu tak, aby mohou číst vlastní nastavení.</span><span class="sxs-lookup"><span data-stu-id="52d0c-271">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="52d0c-272">Více konfigurací může být načten voláním `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` znovu s jinou část.</span><span class="sxs-lookup"><span data-stu-id="52d0c-272">Multiple configurations may be loaded by calling `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="52d0c-273">Pouze poslední konfigurace se použije, pokud `Load` je explicitně zavolán v předchozích instancí.</span><span class="sxs-lookup"><span data-stu-id="52d0c-273">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="52d0c-274">Microsoft.aspnetcore.all nevolá `Load` tak, aby jeho výchozí konfigurační oddíl může být nahrazen.</span><span class="sxs-lookup"><span data-stu-id="52d0c-274">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="52d0c-275">`KestrelConfigurationLoader` zrcadlení `Listen` rozhraní API z řady `KestrelServerOptions` jako `Endpoint` přetížení, takže koncové body kódu a konfiguračním může být nakonfigurována na stejném místě.</span><span class="sxs-lookup"><span data-stu-id="52d0c-275">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="52d0c-276">Tato přetížení nepoužívejte názvy a využívat pouze výchozí nastavení z konfigurace.</span><span class="sxs-lookup"><span data-stu-id="52d0c-276">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="52d0c-277">*Změnit výchozí nastavení v kódu*</span><span class="sxs-lookup"><span data-stu-id="52d0c-277">*Change the defaults in code*</span></span>

<span data-ttu-id="52d0c-278">`ConfigureEndpointDefaults` a `ConfigureHttpsDefaults` můžete použít ke změně výchozího nastavení pro `ListenOptions` a `HttpsConnectionAdapterOptions`, včetně přepisuje výchozí certifikát uvedený v předchozím scénáři.</span><span class="sxs-lookup"><span data-stu-id="52d0c-278">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="52d0c-279">`ConfigureEndpointDefaults` a `ConfigureHttpsDefaults` by měla být volána před všechny koncové body se konfigurují.</span><span class="sxs-lookup"><span data-stu-id="52d0c-279">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="52d0c-280">*Podpora kestrel SNI*</span><span class="sxs-lookup"><span data-stu-id="52d0c-280">*Kestrel support for SNI*</span></span>

<span data-ttu-id="52d0c-281">[Indikace názvu serveru (SNI)](https://tools.ietf.org/html/rfc6066#section-3) lze použít k hostování více domén na stejné IP adrese a portu.</span><span class="sxs-lookup"><span data-stu-id="52d0c-281">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="52d0c-282">Pro SNI na funkci klient odešle název hostitele pro zabezpečenou relaci na server během provádění metody handshake TLS server může poskytnout správný certifikát.</span><span class="sxs-lookup"><span data-stu-id="52d0c-282">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="52d0c-283">Klient použije certifikát zařízená šifrovanou komunikaci se serverem během zabezpečené relace, který následuje TLS handshake.</span><span class="sxs-lookup"><span data-stu-id="52d0c-283">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="52d0c-284">Podporuje SNI přes kestrel `ServerCertificateSelector` zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="52d0c-284">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="52d0c-285">Zpětné volání je vyvolat jednou za připojení, aby mohla aplikace ke kontrole názvu hostitele a vyberte příslušný certifikát.</span><span class="sxs-lookup"><span data-stu-id="52d0c-285">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="52d0c-286">Podpora SNI vyžaduje:</span><span class="sxs-lookup"><span data-stu-id="52d0c-286">SNI support requires:</span></span>

* <span data-ttu-id="52d0c-287">Běží na rozhraní .NET framework `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="52d0c-287">Running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="52d0c-288">Na `netcoreapp2.0` a `net461`, zpětné volání vyvolat ale `name` je vždy `null`.</span><span class="sxs-lookup"><span data-stu-id="52d0c-288">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="52d0c-289">`name` Je také `null` Pokud klienta neobsahuje hostitelský název parametru v TLS handshake.</span><span class="sxs-lookup"><span data-stu-id="52d0c-289">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="52d0c-290">Spustit všechny weby na stejnou instanci Kestrel.</span><span class="sxs-lookup"><span data-stu-id="52d0c-290">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="52d0c-291">Kestrel nepodporuje sdílení IP adresu a port ve více instancích bez reverzního proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="52d0c-291">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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
                var certs = new Dictionary(StringComparer.OrdinalIgnoreCase);
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

<span data-ttu-id="52d0c-292">**Vytvoření vazby na soket TCP**</span><span class="sxs-lookup"><span data-stu-id="52d0c-292">**Bind to a TCP socket**</span></span>

<span data-ttu-id="52d0c-293">[Naslouchání](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) metoda vytvoří vazbu na soket TCP a umožňuje lambda s možností konfigurace certifikátu SSL:</span><span class="sxs-lookup"><span data-stu-id="52d0c-293">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits SSL certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="52d0c-294">Tento příklad konfiguruje SSL pro koncový bod s [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span><span class="sxs-lookup"><span data-stu-id="52d0c-294">The example configures SSL for an endpoint with [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="52d0c-295">Nakonfigurujte další nastavení Kestrel pro konkrétní koncové body pomocí stejného rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="52d0c-295">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

<span data-ttu-id="52d0c-296">**Vytvoření vazby k Unixovému soketu**</span><span class="sxs-lookup"><span data-stu-id="52d0c-296">**Bind to a Unix socket**</span></span>

<span data-ttu-id="52d0c-297">Naslouchání soketu Unix s [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) dosáhnout tak vyššího výkonu se serverem Nginx, jak je znázorněno v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="52d0c-297">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="52d0c-298">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="52d0c-298">**Port 0**</span></span>

<span data-ttu-id="52d0c-299">Když číslo portu `0` určena Kestrel dynamicky váže dostupný port.</span><span class="sxs-lookup"><span data-stu-id="52d0c-299">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="52d0c-300">Následující příklad ukazuje, jak určit port, který Kestrel vázán skutečně za běhu:</span><span class="sxs-lookup"><span data-stu-id="52d0c-300">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="52d0c-301">Při spuštění aplikace Určuje výstup okna konzoly dynamický port, kde se dá kontaktovat aplikace:</span><span class="sxs-lookup"><span data-stu-id="52d0c-301">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

<span data-ttu-id="52d0c-302">**Konfigurační klíč a omezení proměnnou prostředí ASPNETCORE_URLS hostitele UseUrls argument příkazového řádku--adresy URL, adresy URL**</span><span class="sxs-lookup"><span data-stu-id="52d0c-302">**UseUrls, --urls command-line argument, urls host configuration key, and ASPNETCORE_URLS environment variable limitations**</span></span>

<span data-ttu-id="52d0c-303">Konfigurace koncových bodů pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="52d0c-303">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="52d0c-304">UseUrls</span><span class="sxs-lookup"><span data-stu-id="52d0c-304">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="52d0c-305">`--urls` argument příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="52d0c-305">`--urls` command-line argument</span></span>
* <span data-ttu-id="52d0c-306">`urls` Konfigurační klíč hostitele</span><span class="sxs-lookup"><span data-stu-id="52d0c-306">`urls` host configuration key</span></span>
* <span data-ttu-id="52d0c-307">`ASPNETCORE_URLS` Proměnná prostředí</span><span class="sxs-lookup"><span data-stu-id="52d0c-307">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="52d0c-308">Tyto metody jsou užitečné pro provádění kódu práce se servery než Kestrel.</span><span class="sxs-lookup"><span data-stu-id="52d0c-308">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="52d0c-309">Mějte ale na tato omezení:</span><span class="sxs-lookup"><span data-stu-id="52d0c-309">However, be aware of these limitations:</span></span>

* <span data-ttu-id="52d0c-310">Protokol SSL nelze použít s těchto přístupů, pokud výchozí certifikát je součástí konfigurace koncového bodu HTTPS (například pomocí `KestrelServerOptions` konfigurace nebo konfiguračního souboru, jak je znázorněno výše v tomto tématu).</span><span class="sxs-lookup"><span data-stu-id="52d0c-310">SSL can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="52d0c-311">Při i `Listen` a `UseUrls` přístupy se využívat současně, `Listen` přepsat koncových bodů `UseUrls` koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="52d0c-311">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="52d0c-312">**Konfigurace koncového bodu služby IIS**</span><span class="sxs-lookup"><span data-stu-id="52d0c-312">**IIS endpoint configuration**</span></span>

<span data-ttu-id="52d0c-313">Při použití služby IIS, adresa URL vazeb pro služby IIS změnit vazby jsou nastaveny buď `Listen` nebo `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="52d0c-313">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="52d0c-314">Další informace najdete v tématu [modul ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) tématu.</span><span class="sxs-lookup"><span data-stu-id="52d0c-314">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="52d0c-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="52d0c-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="52d0c-316">Ve výchozím nastavení, ASP.NET Core váže k `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="52d0c-316">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="52d0c-317">Konfigurace předpony adres URL a portů pro Kestrel pomocí:</span><span class="sxs-lookup"><span data-stu-id="52d0c-317">Configure URL prefixes and ports for Kestrel using:</span></span>

* <span data-ttu-id="52d0c-318">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) – metoda rozšíření</span><span class="sxs-lookup"><span data-stu-id="52d0c-318">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) extension method</span></span>
* <span data-ttu-id="52d0c-319">`--urls` argument příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="52d0c-319">`--urls` command-line argument</span></span>
* <span data-ttu-id="52d0c-320">`urls` Konfigurační klíč hostitele</span><span class="sxs-lookup"><span data-stu-id="52d0c-320">`urls` host configuration key</span></span>
* <span data-ttu-id="52d0c-321">ASP.NET Core systém konfigurace, včetně `ASPNETCORE_URLS` proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="52d0c-321">ASP.NET Core configuration system, including `ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="52d0c-322">Další informace o těchto metodách, naleznete v tématu [Hosting](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="52d0c-322">For more information on these methods, see [Hosting](xref:fundamentals/host/index).</span></span>

<span data-ttu-id="52d0c-323">**Konfigurace koncového bodu služby IIS**</span><span class="sxs-lookup"><span data-stu-id="52d0c-323">**IIS endpoint configuration**</span></span>

<span data-ttu-id="52d0c-324">Při použití služby IIS, adresa URL vazby pro službu IIS přepsat vazby nastavil `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="52d0c-324">When using IIS, the URL bindings for IIS override bindings set by `UseUrls`.</span></span> <span data-ttu-id="52d0c-325">Další informace najdete v tématu [modul ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) tématu.</span><span class="sxs-lookup"><span data-stu-id="52d0c-325">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

---

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a><span data-ttu-id="52d0c-326">Konfigurace přenosu</span><span class="sxs-lookup"><span data-stu-id="52d0c-326">Transport configuration</span></span>

<span data-ttu-id="52d0c-327">Verze technologie ASP.NET Core 2.1 Kestrel pro výchozí přenos je již podle Libuv ale místo toho podle spravované sokety.</span><span class="sxs-lookup"><span data-stu-id="52d0c-327">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="52d0c-328">Toto je zásadní změny pro aplikace ASP.NET Core 2.0 upgrade na verzi 2.1, které volají [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) a závisí na jednu z následujících balíčků:</span><span class="sxs-lookup"><span data-stu-id="52d0c-328">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) and depend on either of the following packages:</span></span>

* <span data-ttu-id="52d0c-329">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (přímý odkaz na balíček)</span><span class="sxs-lookup"><span data-stu-id="52d0c-329">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="52d0c-330">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="52d0c-330">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="52d0c-331">Pro ASP.NET Core 2.1 nebo novější projekty, které používají [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) a vyžadují použití Libuv:</span><span class="sxs-lookup"><span data-stu-id="52d0c-331">For ASP.NET Core 2.1 or later projects that use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and require the use of Libuv:</span></span>

* <span data-ttu-id="52d0c-332">Přidat závislost [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) balíček do souboru projektu vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="52d0c-332">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="2.1.0" />
    ```

* <span data-ttu-id="52d0c-333">Volání [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span><span class="sxs-lookup"><span data-stu-id="52d0c-333">Call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="52d0c-334">Předpony adres URL</span><span class="sxs-lookup"><span data-stu-id="52d0c-334">URL prefixes</span></span>

<span data-ttu-id="52d0c-335">Při použití `UseUrls`, `--urls` argument příkazového řádku, `urls` konfigurační klíč hostitele, nebo `ASPNETCORE_URLS` proměnné prostředí, předpony adres URL může být v některém z následujících formátů.</span><span class="sxs-lookup"><span data-stu-id="52d0c-335">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="52d0c-336">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="52d0c-336">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="52d0c-337">Platné jsou pouze předpony adres URL protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="52d0c-337">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="52d0c-338">Kestrel nepodporuje SSL při konfiguraci adresy URL vazby, které používají `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="52d0c-338">Kestrel doesn't support SSL when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="52d0c-339">Adresu IPv4 s číslem portu</span><span class="sxs-lookup"><span data-stu-id="52d0c-339">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="52d0c-340">`0.0.0.0` je zvláštní případ s vazbou na všechny adresy IPv4.</span><span class="sxs-lookup"><span data-stu-id="52d0c-340">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="52d0c-341">Adresa protokolu IPv6 s číslem portu</span><span class="sxs-lookup"><span data-stu-id="52d0c-341">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="52d0c-342">`[::]` je ekvivalentem IPv6 IPv4 `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="52d0c-342">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="52d0c-343">Název hostitele s číslem portu</span><span class="sxs-lookup"><span data-stu-id="52d0c-343">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="52d0c-344">Názvy hostitelů `*`, a `+`, nejsou speciální.</span><span class="sxs-lookup"><span data-stu-id="52d0c-344">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="52d0c-345">Nic není rozpoznán jako platná IP adresa nebo `localhost` vytvoří vazbu pro všechny IP adresy IPv6 a IPv4.</span><span class="sxs-lookup"><span data-stu-id="52d0c-345">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="52d0c-346">Pro vázání názvů jiného hostitele a různé aplikace ASP.NET Core na stejném portu, použijte [HTTP.sys](xref:fundamentals/servers/httpsys) nebo reverzní proxy server, jako je například Apache, IIS nebo Nginx.</span><span class="sxs-lookup"><span data-stu-id="52d0c-346">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="52d0c-347">Pokud nepoužíváte reverzního proxy serveru s hostitelem filtrování povolené, povolte [hostitele filtrování](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="52d0c-347">If not using a reverse proxy with host filtering enabled, enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="52d0c-348">Hostitel `localhost` název portu s číslem portu číslo nebo adresu zpětné smyčky IP</span><span class="sxs-lookup"><span data-stu-id="52d0c-348">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="52d0c-349">Když `localhost` určena Kestrel pokusí o připojení k rozhraní zpětné smyčky protokolu IPv4 a IPv6.</span><span class="sxs-lookup"><span data-stu-id="52d0c-349">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="52d0c-350">Pokud požadovaný port je používán jinou službou buď rozhraní zpětné smyčky, Kestrel nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="52d0c-350">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="52d0c-351">Pokud je buď rozhraní zpětné smyčky není k dispozici z jiného důvodu (většinu běžně, protože protokol IPv6 není podporován), Kestrel protokoluje upozornění.</span><span class="sxs-lookup"><span data-stu-id="52d0c-351">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="52d0c-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="52d0c-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="52d0c-353">Adresu IPv4 s číslem portu</span><span class="sxs-lookup"><span data-stu-id="52d0c-353">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="52d0c-354">`0.0.0.0` je zvláštní případ s vazbou na všechny adresy IPv4.</span><span class="sxs-lookup"><span data-stu-id="52d0c-354">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="52d0c-355">Adresa protokolu IPv6 s číslem portu</span><span class="sxs-lookup"><span data-stu-id="52d0c-355">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  <span data-ttu-id="52d0c-356">`[::]` je ekvivalentem IPv6 IPv4 `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="52d0c-356">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="52d0c-357">Název hostitele s číslem portu</span><span class="sxs-lookup"><span data-stu-id="52d0c-357">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="52d0c-358">Názvy hostitelů `*`, a `+` nejsou speciální.</span><span class="sxs-lookup"><span data-stu-id="52d0c-358">Host names, `*`, and `+` aren't special.</span></span> <span data-ttu-id="52d0c-359">Cokoli, co není rozpoznaný IP adresu nebo `localhost` vytvoří vazbu pro všechny IP adresy IPv6 a IPv4.</span><span class="sxs-lookup"><span data-stu-id="52d0c-359">Anything that isn't a recognized IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="52d0c-360">Pro vázání názvů jiného hostitele a různé aplikace ASP.NET Core na stejném portu, použijte [WebListener](xref:fundamentals/servers/weblistener) nebo reverzní proxy server, jako je například Apache, IIS nebo Nginx.</span><span class="sxs-lookup"><span data-stu-id="52d0c-360">To bind different host names to different ASP.NET Core apps on the same port, use [WebListener](xref:fundamentals/servers/weblistener) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="52d0c-361">Hostitel `localhost` název portu s číslem portu číslo nebo adresu zpětné smyčky IP</span><span class="sxs-lookup"><span data-stu-id="52d0c-361">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="52d0c-362">Když `localhost` určena Kestrel pokusí o připojení k rozhraní zpětné smyčky protokolu IPv4 a IPv6.</span><span class="sxs-lookup"><span data-stu-id="52d0c-362">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="52d0c-363">Pokud požadovaný port je používán jinou službou buď rozhraní zpětné smyčky, Kestrel nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="52d0c-363">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="52d0c-364">Pokud je buď rozhraní zpětné smyčky není k dispozici z jiného důvodu (většinu běžně, protože protokol IPv6 není podporován), Kestrel protokoluje upozornění.</span><span class="sxs-lookup"><span data-stu-id="52d0c-364">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

* <span data-ttu-id="52d0c-365">Unixovému soketu</span><span class="sxs-lookup"><span data-stu-id="52d0c-365">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock
  ```

<span data-ttu-id="52d0c-366">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="52d0c-366">**Port 0**</span></span>

<span data-ttu-id="52d0c-367">Pokud je číslo portu `0` určena Kestrel dynamicky váže dostupný port.</span><span class="sxs-lookup"><span data-stu-id="52d0c-367">When the port number is `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="52d0c-368">Vytvoření vazby na port `0` povoleny pro název hostitele nebo IP adresy s výjimkou pro `localhost`.</span><span class="sxs-lookup"><span data-stu-id="52d0c-368">Binding to port `0` is allowed for any host name or IP except for `localhost`.</span></span>

<span data-ttu-id="52d0c-369">Při spuštění aplikace Určuje výstup okna konzoly dynamický port, kde se dá kontaktovat aplikace:</span><span class="sxs-lookup"><span data-stu-id="52d0c-369">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="52d0c-370">**Předpony adres URL pro protokol SSL**</span><span class="sxs-lookup"><span data-stu-id="52d0c-370">**URL prefixes for SSL**</span></span>

<span data-ttu-id="52d0c-371">Pokud volání `UseHttps` rozšiřující metoda ji nezapomeňte zahrnout předpony adres URL s `https:`:</span><span class="sxs-lookup"><span data-stu-id="52d0c-371">If calling the `UseHttps` extension method, be sure to include URL prefixes with `https:`:</span></span>

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
> <span data-ttu-id="52d0c-372">Na stejném portu nemůže být hostovaná protokolů HTTPS a HTTP.</span><span class="sxs-lookup"><span data-stu-id="52d0c-372">HTTPS and HTTP can't be hosted on the same port.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

---

## <a name="host-filtering"></a><span data-ttu-id="52d0c-373">Hostitel filtrování</span><span class="sxs-lookup"><span data-stu-id="52d0c-373">Host filtering</span></span>

<span data-ttu-id="52d0c-374">I když Kestrel podporuje konfigurace, například podle předpon `http://example.com:5000`, Kestrel do značné míry ignoruje název hostitele.</span><span class="sxs-lookup"><span data-stu-id="52d0c-374">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="52d0c-375">Hostitel `localhost` je zvláštní případ použité pro vazbu na adresu zpětné smyčky adresy.</span><span class="sxs-lookup"><span data-stu-id="52d0c-375">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="52d0c-376">Všechny hostitele, jiné než explicitních IP adresu vytvoří vazbu na všechny veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="52d0c-376">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="52d0c-377">Žádná z těchto informací slouží k ověření požadavku `Host` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="52d0c-377">None of this information is used to validate request `Host` headers.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="52d0c-378">Alternativním řešením je hostitelem za reverzní proxy server s filtrováním hlavičky hostitele.</span><span class="sxs-lookup"><span data-stu-id="52d0c-378">As a workaround, host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="52d0c-379">Toto je jediný podporovaný scénář pro Kestrel v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="52d0c-379">This is the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="52d0c-380">Jako alternativní řešení použít k filtrování požadavků podle middlewaru `Host` hlavičky:</span><span class="sxs-lookup"><span data-stu-id="52d0c-380">As a workaround, use middleware to filter requests by the `Host` header:</span></span>

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

<span data-ttu-id="52d0c-381">Zaregistrujte předchozí `HostFilteringMiddleware` v `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="52d0c-381">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="52d0c-382">Všimněte si, že [řazení zápisu middleware](xref:fundamentals/middleware/index#ordering) je důležité.</span><span class="sxs-lookup"><span data-stu-id="52d0c-382">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#ordering) is important.</span></span> <span data-ttu-id="52d0c-383">Registrace se budou objevovat hned po registraci diagnostických Middleware (například `app.UseExceptionHandler`).</span><span class="sxs-lookup"><span data-stu-id="52d0c-383">Registration should occur immediately after Diagnostic Middleware registration (for example, `app.UseExceptionHandler`).</span></span>

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

<span data-ttu-id="52d0c-384">Middleware očekává, že `AllowedHosts` klíče v *appsettings.json*/*appsettings.\< EnvironmentName > .json*.</span><span class="sxs-lookup"><span data-stu-id="52d0c-384">The middleware expects an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="52d0c-385">Hodnota je středníkem oddělený seznam názvů hostitele bez čísla portů:</span><span class="sxs-lookup"><span data-stu-id="52d0c-385">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="52d0c-386">Jako alternativní řešení použijte hostitele filtrování middlewaru.</span><span class="sxs-lookup"><span data-stu-id="52d0c-386">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="52d0c-387">Poskytuje Middleware filtrování hostitele [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) balíček, který je součástí [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="52d0c-387">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="52d0c-388">Přidá middleware [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), který volá [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span><span class="sxs-lookup"><span data-stu-id="52d0c-388">The middleware is added by [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="52d0c-389">Middleware filtrování hostitele je ve výchozím nastavení zakázána.</span><span class="sxs-lookup"><span data-stu-id="52d0c-389">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="52d0c-390">Chcete-li povolit middleware, definujte `AllowedHosts` klíče v *appsettings.json*/*appsettings.\< EnvironmentName > .json*.</span><span class="sxs-lookup"><span data-stu-id="52d0c-390">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="52d0c-391">Hodnota je středníkem oddělený seznam názvů hostitele bez čísla portů:</span><span class="sxs-lookup"><span data-stu-id="52d0c-391">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="52d0c-392">*appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="52d0c-392">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="52d0c-393">[Předané záhlaví Middleware](xref:host-and-deploy/proxy-load-balancer) má také [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) možnost.</span><span class="sxs-lookup"><span data-stu-id="52d0c-393">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) option.</span></span> <span data-ttu-id="52d0c-394">Přesměrovaná záhlaví Middleware a filtrování Middleware hostitele mají podobné funkce pro různé scénáře.</span><span class="sxs-lookup"><span data-stu-id="52d0c-394">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="52d0c-395">Nastavení `AllowedHosts` předané Middleware hlavičky je vhodné při hlavičku hostitele nezachová při předávání žádostí s reverzní proxy server nebo nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="52d0c-395">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the Host header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="52d0c-396">Nastavení `AllowedHosts` s Middlewarem filtrování hostitele je vhodné při Kestrel slouží jako edge server nebo když je hlavička hostitele přímo předán.</span><span class="sxs-lookup"><span data-stu-id="52d0c-396">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as an edge server or when the Host header is directly forwarded.</span></span>
>
> <span data-ttu-id="52d0c-397">Další informace o předávaných Middleware záhlaví, naleznete v tématu [konfigurace ASP.NET Core práci se servery proxy a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="52d0c-397">For more information on Forwarded Headers Middleware, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="52d0c-398">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="52d0c-398">Additional resources</span></span>

* [<span data-ttu-id="52d0c-399">Vynucení protokolu HTTPS</span><span class="sxs-lookup"><span data-stu-id="52d0c-399">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="52d0c-400">Kestrel zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="52d0c-400">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
* [<span data-ttu-id="52d0c-401">RFC 7230: Syntaxe a směrování zpráv (oddíl 5.4: hostitele)</span><span class="sxs-lookup"><span data-stu-id="52d0c-401">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
* [<span data-ttu-id="52d0c-402">Konfigurace ASP.NET Core práci se servery proxy a nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="52d0c-402">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
