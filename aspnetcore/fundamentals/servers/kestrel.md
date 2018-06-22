---
title: Kestrel webového serveru implementace v ASP.NET Core
author: rick-anderson
description: Další informace o Kestrel napříč platformami webovém serveru pro ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/02/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 62649351271deebcf1ed9d2f8b2258bed3478989
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276653"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="22bff-103">Kestrel webového serveru implementace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="22bff-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="22bff-104">Podle [tní Dykstra](https://github.com/tdykstra), [Jan Ross](https://github.com/Tratcher), a [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="22bff-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="22bff-105">Kestrel je napříč platformami [webového serveru pro ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="22bff-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="22bff-106">Kestrel je webový server, který je zahrnut ve výchozím nastavení v šablony projektů ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="22bff-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="22bff-107">Kestrel podporuje následující funkce:</span><span class="sxs-lookup"><span data-stu-id="22bff-107">Kestrel supports the following features:</span></span>

* <span data-ttu-id="22bff-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="22bff-108">HTTPS</span></span>
* <span data-ttu-id="22bff-109">Neprůhledné upgrade používaným pro povolení [objekty WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="22bff-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="22bff-110">Sokety UNIX pro vysoký výkon za Nginx</span><span class="sxs-lookup"><span data-stu-id="22bff-110">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="22bff-111">Kestrel je podporována na všech platformách a verze, které podporuje .NET Core.</span><span class="sxs-lookup"><span data-stu-id="22bff-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="22bff-112">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="22bff-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="22bff-113">Kdy použít Kestrel s reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="22bff-113">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22bff-114">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="22bff-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="22bff-115">Kestrel můžete použít samostatně nebo se *reverzní proxy server*, jako jsou například služby IIS, Nginx nebo Apache.</span><span class="sxs-lookup"><span data-stu-id="22bff-115">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="22bff-116">Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování.</span><span class="sxs-lookup"><span data-stu-id="22bff-116">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel komunikuje přímo s Internetu bez reverzní proxy server](kestrel/_static/kestrel-to-internet2.png)

![Kestrel nepřímo komunikuje v Internetu přes reverzní proxy server, například služby IIS, Nginx nebo Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="22bff-119">Buď konfiguraci&mdash;s nebo bez reverzní proxy server&mdash;je platný a podporované konfigurace hostování pro technologii ASP.NET Core 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="22bff-119">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22bff-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22bff-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="22bff-121">Pokud aplikace přijímá požadavky jenom z interní sítě, Kestrel můžete použít přímo jako server aplikace.</span><span class="sxs-lookup"><span data-stu-id="22bff-121">If an app accepts requests only from an internal network, Kestrel can be used directly as the app's server.</span></span>

![Kestrel komunikuje přímo s interní sítě](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="22bff-123">Pokud vystavit aplikace k Internetu, použít službu IIS, Nginx nebo Apache jako *reverzní proxy server*.</span><span class="sxs-lookup"><span data-stu-id="22bff-123">If you expose your app to the Internet, use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="22bff-124">Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování.</span><span class="sxs-lookup"><span data-stu-id="22bff-124">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel nepřímo komunikuje v Internetu přes reverzní proxy server, například služby IIS, Nginx nebo Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="22bff-126">Reverzní proxy server je vyžadována pro nasazení okraj (vystavený pro přenosy z Internetu) z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="22bff-126">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="22bff-127">Verze 1.x Kestrel nemají úplný doplněk obrany před útoky, například odpovídající vypršení časových limitů, omezení velikosti a omezení počtu souběžných připojení.</span><span class="sxs-lookup"><span data-stu-id="22bff-127">The 1.x versions of Kestrel don't have a full complement of defenses against attacks, such as appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="22bff-128">Scénář reverzní proxy server existuje, pokud existuje víc aplikací, které sdílejí stejnou adresu IP a portu spouští na jednom serveru.</span><span class="sxs-lookup"><span data-stu-id="22bff-128">A reverse proxy scenario exists when there are multiple apps that share the same IP and port running on a single server.</span></span> <span data-ttu-id="22bff-129">Kestrel nepodporuje tento scénář, protože Kestrel nepodporuje sdílení stejné IP adresy a portu mezi více procesů.</span><span class="sxs-lookup"><span data-stu-id="22bff-129">Kestrel doesn't support this scenario because Kestrel doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="22bff-130">Když Kestrel je nakonfigurován pro naslouchání na portu, Kestrel zpracovává veškeré přenosy dat pro tento port bez ohledu na to žádostí Hlavička hostitele.</span><span class="sxs-lookup"><span data-stu-id="22bff-130">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' host header.</span></span> <span data-ttu-id="22bff-131">Reverzní proxy server, můžete sdílet porty má schopnost směrování žádostí Kestrel na jedinečné IP adresy a portu.</span><span class="sxs-lookup"><span data-stu-id="22bff-131">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="22bff-132">I když není požadovaných reverzní proxy server, pomocí reverzní proxy server může být vhodné použít:</span><span class="sxs-lookup"><span data-stu-id="22bff-132">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice:</span></span>

* <span data-ttu-id="22bff-133">Ho můžete omezit zveřejněné veřejnou oblast aplikací, které ji hostuje.</span><span class="sxs-lookup"><span data-stu-id="22bff-133">It can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="22bff-134">Poskytuje další úroveň konfigurace a obrany.</span><span class="sxs-lookup"><span data-stu-id="22bff-134">It provides an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="22bff-135">Může integrovat lépe stávající infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="22bff-135">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="22bff-136">Usnadňují Vyrovnávání zatížení a konfiguraci šifrování SSL.</span><span class="sxs-lookup"><span data-stu-id="22bff-136">It simplifies load balancing and SSL configuration.</span></span> <span data-ttu-id="22bff-137">Pouze reverzní proxy server vyžaduje certifikát SSL a tento server může komunikovat s vašimi aplikací servery v interní síti pomocí prostý protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="22bff-137">Only the reverse proxy server requires an SSL certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="22bff-138">Pokud nepoužíváte reverzní proxy server s hostitelem filtrování povoleno, [hostitele filtrování](#host-filtering) musí být povolena.</span><span class="sxs-lookup"><span data-stu-id="22bff-138">If not using a reverse proxy with host filtering enabled, [host filtering](#host-filtering) must be enabled.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="22bff-139">Jak používat Kestrel v aplikacích ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="22bff-139">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22bff-140">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="22bff-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="22bff-141">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) balíček je součástí [Microsoft.AspNetCore.App metapackage] (xref:fundamentals / metapackage aplikace) (ASP.NET Core 2.1 nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="22bff-141">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage] (xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="22bff-142">Šablony projektů ASP.NET Core pomocí Kestrel ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="22bff-142">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="22bff-143">V *Program.cs*, kód zavolá metodu šablony [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), který volá [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) na pozadí.</span><span class="sxs-lookup"><span data-stu-id="22bff-143">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22bff-144">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22bff-144">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="22bff-145">Nainstalujte [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="22bff-145">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="22bff-146">Volání [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) rozšiřující metody na [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) v `Main` metoda, zadání žádné [Kestrel možnosti](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) potřeba, jak je znázorněno v následujícím oddílu.</span><span class="sxs-lookup"><span data-stu-id="22bff-146">Call the [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) in the `Main` method, specifying any [Kestrel options](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) required, as shown in the next section.</span></span>

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="22bff-147">Možnosti kestrel</span><span class="sxs-lookup"><span data-stu-id="22bff-147">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22bff-148">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="22bff-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="22bff-149">Kestrel webového serveru obsahuje možnosti konfigurace omezení, které jsou užitečné zejména v internetových nasazení.</span><span class="sxs-lookup"><span data-stu-id="22bff-149">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="22bff-150">Několik důležitých omezení, které se dají přizpůsobit:</span><span class="sxs-lookup"><span data-stu-id="22bff-150">A few important limits that can be customized:</span></span>

* <span data-ttu-id="22bff-151">Maximální počet klientských připojení</span><span class="sxs-lookup"><span data-stu-id="22bff-151">Maximum client connections</span></span>
* <span data-ttu-id="22bff-152">Velikost textu maximální požadavku</span><span class="sxs-lookup"><span data-stu-id="22bff-152">Maximum request body size</span></span>
* <span data-ttu-id="22bff-153">Minimální požadavek textu přenosová rychlost</span><span class="sxs-lookup"><span data-stu-id="22bff-153">Minimum request body data rate</span></span>

<span data-ttu-id="22bff-154">Nastavit na tyto a další omezení [omezení](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) vlastnost [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) třídy.</span><span class="sxs-lookup"><span data-stu-id="22bff-154">Set these and other constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="22bff-155">`Limits` Vlastnost obsahuje instanci [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) třídy.</span><span class="sxs-lookup"><span data-stu-id="22bff-155">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

<span data-ttu-id="22bff-156">**Maximální počet klientských připojení**</span><span class="sxs-lookup"><span data-stu-id="22bff-156">**Maximum client connections**</span></span>

[<span data-ttu-id="22bff-157">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="22bff-157">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="22bff-158">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="22bff-158">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

<span data-ttu-id="22bff-159">Maximální počet souběžných otevřete připojení TCP lze nastavit pro celou aplikaci s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="22bff-159">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="22bff-160">Je samostatný limit pro připojení, které byly upgradované z protokolu HTTP nebo HTTPS na jiný protokol (například pro objekty WebSockets požadavek).</span><span class="sxs-lookup"><span data-stu-id="22bff-160">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="22bff-161">Po upgradu připojení není započítané `MaxConcurrentConnections` limit.</span><span class="sxs-lookup"><span data-stu-id="22bff-161">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="22bff-162">Maximální počet připojení je neomezená (null) ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="22bff-162">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="22bff-163">**Velikost textu maximální požadavku**</span><span class="sxs-lookup"><span data-stu-id="22bff-163">**Maximum request body size**</span></span>

[<span data-ttu-id="22bff-164">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="22bff-164">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="22bff-165">Výchozí velikost těla maximální požadavku je 30,000,000 bajtů, což je přibližně 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="22bff-165">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="22bff-166">Doporučeným přístupem k přepsání omezení v aplikaci ASP.NET MVC základní se má používat [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) atribut na metodu akce:</span><span class="sxs-lookup"><span data-stu-id="22bff-166">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="22bff-167">Tady je příklad, který ukazuje, jak nakonfigurovat omezení pro aplikaci na každou žádost:</span><span class="sxs-lookup"><span data-stu-id="22bff-167">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="22bff-168">Můžete přepsat nastavení konkrétního požadavku v middlewaru.:</span><span class="sxs-lookup"><span data-stu-id="22bff-168">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="22bff-169">Když zkusíte nakonfigurovat limit na požadavek po spuštění aplikace přečíst požadavek, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="22bff-169">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="22bff-170">Došlo `IsReadOnly` vlastnost, která určuje, pokud `MaxRequestBodySize` vlastnost je ve stavu jen pro čtení, tzn. je příliš pozdě Konfigurace limitu.</span><span class="sxs-lookup"><span data-stu-id="22bff-170">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="22bff-171">**Minimální požadavek textu přenosová rychlost**</span><span class="sxs-lookup"><span data-stu-id="22bff-171">**Minimum request body data rate**</span></span>

[<span data-ttu-id="22bff-172">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="22bff-172">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="22bff-173">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="22bff-173">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="22bff-174">Pokud data je přicházejících na určenou míru v bajtech za sekundu, zkontroluje kestrel každou sekundu.</span><span class="sxs-lookup"><span data-stu-id="22bff-174">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="22bff-175">Pokud rychlost klesne pod minimální, vypršení časového limitu připojení. Poskytnutá lhůta je množství času, aby Kestrel dává klienta ke zvýšení jeho rychlost odesílání až minimální; rychlost, jakou kontrolována během této doby.</span><span class="sxs-lookup"><span data-stu-id="22bff-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="22bff-176">Období odkladu pomáhá zabránit, vyřazení připojení, které jsou původně odesílání dat s nízkou rychlostí z důvodu zpomalit TCP-start.</span><span class="sxs-lookup"><span data-stu-id="22bff-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="22bff-177">Výchozí minimální rychlost je 240 bajtů za sekundu s období odkladu 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="22bff-177">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="22bff-178">Minimální rychlost platí také pro odpověď.</span><span class="sxs-lookup"><span data-stu-id="22bff-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="22bff-179">Kód pro nastavení limitu požadavků a omezení odpovědi je stejný kromě nutnosti `RequestBody` nebo `Response` v názvech vlastností a rozhraní.</span><span class="sxs-lookup"><span data-stu-id="22bff-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="22bff-180">Tady je příklad, který ukazuje, jak konfigurovat minimální datové sazby v *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="22bff-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-7)]

<span data-ttu-id="22bff-181">Sazby za žádosti můžete nakonfigurovat v middlewaru:</span><span class="sxs-lookup"><span data-stu-id="22bff-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="22bff-182">Informace o dalších možnostech Kestrel a omezení najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="22bff-182">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="22bff-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="22bff-183">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="22bff-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="22bff-184">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="22bff-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="22bff-185">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22bff-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22bff-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="22bff-187">Informace o možnostech Kestrel a omezení najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="22bff-187">For information about Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="22bff-188">KestrelServerOptions – třída</span><span class="sxs-lookup"><span data-stu-id="22bff-188">KestrelServerOptions class</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [<span data-ttu-id="22bff-189">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="22bff-189">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

---

### <a name="endpoint-configuration"></a><span data-ttu-id="22bff-190">Konfigurace koncového bodu</span><span class="sxs-lookup"><span data-stu-id="22bff-190">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22bff-191">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="22bff-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="22bff-192">Ve výchozím nastavení, ASP.NET Core váže k `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="22bff-192">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="22bff-193">Volání [naslouchání](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) nebo [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) metody na [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) konfigurace předpony adres URL a portů pro Kestrel.</span><span class="sxs-lookup"><span data-stu-id="22bff-193">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span> <span data-ttu-id="22bff-194">`UseUrls`, `--urls` argument příkazového řádku, `urls` klíč konfigurace hostitele a `ASPNETCORE_URLS` také pracovní proměnnou prostředí ale mají omezení později uvedených v této části.</span><span class="sxs-lookup"><span data-stu-id="22bff-194">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section.</span></span>

<span data-ttu-id="22bff-195">`urls` Klíč konfigurace hostitele musí pocházet z konfigurace hostitele, není konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="22bff-195">The `urls` host configuration key must come from the host configuration, not the app configuration.</span></span> <span data-ttu-id="22bff-196">Přidání `urls` klíče a hodnoty na *appSettings.JSON určený* konfigurace hostitele nemá vliv, protože hostitel je zcela inicializován v době konfigurace je pro čtení z konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="22bff-196">Adding a `urls` key and value to *appsettings.json* doesn't affect host configuration because the host is completely initialized by the time the configuration is read from the configuration file.</span></span> <span data-ttu-id="22bff-197">Ale `urls` klíče v *appSettings.JSON určený* lze použít s [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) na tvůrce hostitele, který má konfigurace hostitele:</span><span class="sxs-lookup"><span data-stu-id="22bff-197">However, a `urls` key in *appsettings.json* can be used with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) on the host builder to configure the host:</span></span>

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

<span data-ttu-id="22bff-198">Ve výchozím nastavení ASP.NET Core váže k:</span><span class="sxs-lookup"><span data-stu-id="22bff-198">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="22bff-199">`https://localhost:5001` (když místní vývojový certifikát je k dispozici)</span><span class="sxs-lookup"><span data-stu-id="22bff-199">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="22bff-200">Vývojový certifikát se vytvoří:</span><span class="sxs-lookup"><span data-stu-id="22bff-200">A development certificate is created:</span></span>

* <span data-ttu-id="22bff-201">Když [.NET Core SDK](/dotnet/core/sdk) je nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="22bff-201">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="22bff-202">[Dev certifikátů nástroj](xref:aspnetcore-2.1#https) se používá k vytvoření certifikátu.</span><span class="sxs-lookup"><span data-stu-id="22bff-202">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="22bff-203">Některé prohlížeče vyžadují, že udělíte výslovná oprávnění do prohlížeče tak, aby důvěřoval místní vývojový certifikát.</span><span class="sxs-lookup"><span data-stu-id="22bff-203">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="22bff-204">ASP.NET Core 2.1 a novější šablony projektů konfigurace aplikací pro spuštění na HTTPS ve výchozím nastavení a zahrnují [podporují přesměrování protokolu HTTPS a HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="22bff-204">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="22bff-205">Volání [naslouchání](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) nebo [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) metody na [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) konfigurace předpony adres URL a portů pro Kestrel.</span><span class="sxs-lookup"><span data-stu-id="22bff-205">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="22bff-206">`UseUrls`, `--urls` argument příkazového řádku, `urls` klíč konfigurace hostitele a `ASPNETCORE_URLS` také pracovní proměnnou prostředí ale mají omezení později uvedených v této části (výchozí certifikát musí být k dispozici pro koncový bod HTTPS Konfigurace).</span><span class="sxs-lookup"><span data-stu-id="22bff-206">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="22bff-207">ASP.NET Core 2.1 `KestrelServerOptions` konfigurace:</span><span class="sxs-lookup"><span data-stu-id="22bff-207">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

<span data-ttu-id="22bff-208">**ConfigureEndpointDefaults (akce&lt;ListenOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="22bff-208">**ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)**</span></span>  
<span data-ttu-id="22bff-209">Určuje konfiguraci `Action` ke spuštění pro každý zadaný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="22bff-209">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="22bff-210">Volání metody `ConfigureEndpointDefaults` vícekrát nahrazuje před `Action`s s poslední `Action` zadaný.</span><span class="sxs-lookup"><span data-stu-id="22bff-210">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="22bff-211">**ConfigureHttpsDefaults (akce&lt;HttpsConnectionAdapterOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="22bff-211">**ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)**</span></span>  
<span data-ttu-id="22bff-212">Určuje konfiguraci `Action` ke spuštění pro každý koncový bod HTTPS.</span><span class="sxs-lookup"><span data-stu-id="22bff-212">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="22bff-213">Volání metody `ConfigureHttpsDefaults` vícekrát nahrazuje před `Action`s s poslední `Action` zadaný.</span><span class="sxs-lookup"><span data-stu-id="22bff-213">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="22bff-214">**Configure(IConfiguration)**</span><span class="sxs-lookup"><span data-stu-id="22bff-214">**Configure(IConfiguration)**</span></span>  
<span data-ttu-id="22bff-215">Vytvoří zavaděč konfigurace pro nastavení Kestrel, který přebírá [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) jako vstup.</span><span class="sxs-lookup"><span data-stu-id="22bff-215">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="22bff-216">Konfigurace musí být určené ke konfiguračnímu oddílu pro Kestrel.</span><span class="sxs-lookup"><span data-stu-id="22bff-216">The configuration must be scoped to the configuration section for Kestrel.</span></span>

<span data-ttu-id="22bff-217">**ListenOptions.UseHttps**</span><span class="sxs-lookup"><span data-stu-id="22bff-217">**ListenOptions.UseHttps**</span></span>  
<span data-ttu-id="22bff-218">Nakonfigurujte Kestrel pro použití protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="22bff-218">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="22bff-219">`ListenOptions.UseHttps` rozšíření:</span><span class="sxs-lookup"><span data-stu-id="22bff-219">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="22bff-220">`UseHttps` &ndash; Nakonfigurujte Kestrel pro použití protokolu HTTPS s výchozí certifikát.</span><span class="sxs-lookup"><span data-stu-id="22bff-220">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="22bff-221">Vyvolá výjimku, pokud je nakonfigurovaný žádný výchozí certifikát.</span><span class="sxs-lookup"><span data-stu-id="22bff-221">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="22bff-222">`ListenOptions.UseHttps` Parametry:</span><span class="sxs-lookup"><span data-stu-id="22bff-222">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="22bff-223">`filename` je název a cesta k souboru soubor certifikátu, relativně k adresáři, který obsahuje soubory obsahu aplikace.</span><span class="sxs-lookup"><span data-stu-id="22bff-223">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="22bff-224">`password` je požadováno pro přístup k datům certifikát X.509 heslo.</span><span class="sxs-lookup"><span data-stu-id="22bff-224">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="22bff-225">`configureOptions` je `Action` nakonfigurovat `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="22bff-225">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="22bff-226">Vrátí `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="22bff-226">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="22bff-227">`storeName` je úložiště certifikátů pro načtení certifikátu.</span><span class="sxs-lookup"><span data-stu-id="22bff-227">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="22bff-228">`subject` je název subjektu certifikátu.</span><span class="sxs-lookup"><span data-stu-id="22bff-228">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="22bff-229">`allowInvalid` Označuje, pokud neplatný certifikáty musí vzít v úvahu například certifikáty podepsané svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="22bff-229">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="22bff-230">`location` je načíst certifikát z umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="22bff-230">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="22bff-231">`serverCertificate` je certifikát X.509.</span><span class="sxs-lookup"><span data-stu-id="22bff-231">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="22bff-232">V produkčním prostředí musí být explicitně nakonfigurovaný protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="22bff-232">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="22bff-233">Minimálně je třeba zadat výchozí certifikát.</span><span class="sxs-lookup"><span data-stu-id="22bff-233">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="22bff-234">Podporované konfigurace popsána dále:</span><span class="sxs-lookup"><span data-stu-id="22bff-234">Supported configurations described next:</span></span>

* <span data-ttu-id="22bff-235">Žádná konfigurace</span><span class="sxs-lookup"><span data-stu-id="22bff-235">No configuration</span></span>
* <span data-ttu-id="22bff-236">Nahraďte výchozí certifikát z konfigurace</span><span class="sxs-lookup"><span data-stu-id="22bff-236">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="22bff-237">Změnit výchozí nastavení v kódu</span><span class="sxs-lookup"><span data-stu-id="22bff-237">Change the defaults in code</span></span>

<span data-ttu-id="22bff-238">*Žádná konfigurace*</span><span class="sxs-lookup"><span data-stu-id="22bff-238">*No configuration*</span></span>

<span data-ttu-id="22bff-239">Kestrel naslouchá na `http://localhost:5000` a `https://localhost:5001` (Pokud je výchozí certifikát je k dispozici).</span><span class="sxs-lookup"><span data-stu-id="22bff-239">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="22bff-240">Určení adres URL pomocí:</span><span class="sxs-lookup"><span data-stu-id="22bff-240">Specify URLs using the:</span></span>

* <span data-ttu-id="22bff-241">`ASPNETCORE_URLS` Proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="22bff-241">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="22bff-242">`--urls` argument příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="22bff-242">`--urls` command-line argument.</span></span>
* <span data-ttu-id="22bff-243">`urls` Klíč konfigurace hostitele.</span><span class="sxs-lookup"><span data-stu-id="22bff-243">`urls` host configuration key.</span></span>
* <span data-ttu-id="22bff-244">`UseUrls` metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="22bff-244">`UseUrls` extension method.</span></span>

<span data-ttu-id="22bff-245">Další informace najdete v tématu [adresy URL serveru](xref:fundamentals/host/web-host#server-urls) a [konfigurace přepisování](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="22bff-245">For more information, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="22bff-246">Hodnota zadaná pomocí těchto přístupů může být jeden nebo více HTTP a HTTPS koncových bodů (HTTPS Pokud je k dispozici certifikát výchozí).</span><span class="sxs-lookup"><span data-stu-id="22bff-246">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="22bff-247">Nakonfigurovat hodnotu jako seznam oddělený středníkem (například `"Urls": "http://localhost:8000;http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="22bff-247">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="22bff-248">*Nahraďte výchozí certifikát z konfigurace*</span><span class="sxs-lookup"><span data-stu-id="22bff-248">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="22bff-249">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) volání `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` ve výchozím nastavení se načíst konfiguraci Kestrel.</span><span class="sxs-lookup"><span data-stu-id="22bff-249">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="22bff-250">Schéma výchozí HTTPS aplikace nastavení konfigurace je k dispozici pro Kestrel.</span><span class="sxs-lookup"><span data-stu-id="22bff-250">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="22bff-251">Nakonfigurujte několik koncových bodů, včetně adres URL a certifikátů, které chcete použít, ze souboru na disku nebo z úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="22bff-251">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="22bff-252">V následujícím *appSettings.JSON určený* příklad:</span><span class="sxs-lookup"><span data-stu-id="22bff-252">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="22bff-253">Nastavit **AllowInvalid** k `true` k povolení použití neplatný certifikátů (například certifikáty podepsané svým držitelem).</span><span class="sxs-lookup"><span data-stu-id="22bff-253">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="22bff-254">Žádný koncový bod HTTPS, který není určete certifikát (**HttpsDefaultCert** v příkladu, který následuje) spadne zpět na cert definované v části **certifikáty** > **výchozí**  nebo vývojový certifikát.</span><span class="sxs-lookup"><span data-stu-id="22bff-254">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="22bff-255">Alternativu k použití **cesta** a **heslo** pro jakýkoliv certifikát je uzel zadejte certifikát pomocí polí úložiště certifikátu.</span><span class="sxs-lookup"><span data-stu-id="22bff-255">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="22bff-256">Například **certifikáty** > **výchozí** certifikát lze zadat jako:</span><span class="sxs-lookup"><span data-stu-id="22bff-256">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="22bff-257">Schéma poznámky:</span><span class="sxs-lookup"><span data-stu-id="22bff-257">Schema notes:</span></span>

* <span data-ttu-id="22bff-258">Názvy koncových bodů jsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="22bff-258">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="22bff-259">Například `HTTPS` a `Https` jsou platné.</span><span class="sxs-lookup"><span data-stu-id="22bff-259">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="22bff-260">`Url` Parametr je vyžadován pro každý koncový bod.</span><span class="sxs-lookup"><span data-stu-id="22bff-260">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="22bff-261">Formát pro tento parametr je stejný jako nejvyšší úrovně `Urls` parametr konfigurace s výjimkou, že je omezena na jednu hodnotu.</span><span class="sxs-lookup"><span data-stu-id="22bff-261">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="22bff-262">Tyto koncové body nahradit názvům definovaným v nejvyšší úrovně `Urls` konfigurace spíše než přidávání na ně.</span><span class="sxs-lookup"><span data-stu-id="22bff-262">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="22bff-263">Koncové body definované v kódu prostřednictvím `Listen` jsou kumulativní s koncovými body definované v konfiguračním oddílu.</span><span class="sxs-lookup"><span data-stu-id="22bff-263">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="22bff-264">`Certificate` Část je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="22bff-264">The `Certificate` section is optional.</span></span> <span data-ttu-id="22bff-265">Pokud `Certificate` části nezadáte, budou použity výchozí hodnoty definované v předchozích scénáře.</span><span class="sxs-lookup"><span data-stu-id="22bff-265">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="22bff-266">Pokud jsou k dispozici žádné výchozí hodnoty, serveru vyvolá výjimku a nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="22bff-266">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="22bff-267">`Certificate` Části podporuje obě **cesta**&ndash;**heslo** a **subjektu**&ndash;**úložiště** certifikáty.</span><span class="sxs-lookup"><span data-stu-id="22bff-267">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="22bff-268">Tímto způsobem může být definována libovolný počet koncových bodů, tak dlouho, dokud se nezpůsobí, že port je v konfliktu.</span><span class="sxs-lookup"><span data-stu-id="22bff-268">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="22bff-269">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` Vrátí `KestrelConfigurationLoader` s `.Endpoint(string name, options => { })` metoda, která umožňuje doplnit nakonfigurovaná nastavení pro koncový bod:</span><span class="sxs-lookup"><span data-stu-id="22bff-269">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="22bff-270">Můžete rovněž přímo přistupovat ke `KestrelServerOptions.ConfigurationLoader` zachovat procházení na existující zavaděč, jako je třeba poskytované [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="22bff-270">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

* <span data-ttu-id="22bff-271">Konfigurační oddíl pro každý koncový bod je k dispozici na možnosti v `Endpoint` metoda tak, aby mohou číst vlastní nastavení.</span><span class="sxs-lookup"><span data-stu-id="22bff-271">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="22bff-272">Více konfigurací může načíst voláním `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` znovu s jiným oddílem.</span><span class="sxs-lookup"><span data-stu-id="22bff-272">Multiple configurations may be loaded by calling `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="22bff-273">Pouze poslední konfigurace se používá, pokud `Load` je explicitně volána na předchozí instance.</span><span class="sxs-lookup"><span data-stu-id="22bff-273">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="22bff-274">Není volání metapackage `Load` tak, aby jeho výchozí konfigurační oddíl lze nahradit.</span><span class="sxs-lookup"><span data-stu-id="22bff-274">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="22bff-275">`KestrelConfigurationLoader` zrcadlení `Listen` rodiny rozhraní API z `KestrelServerOptions` jako `Endpoint` přetížení, takže kód a konfigurace koncových bodů může být nakonfigurována na stejném místě.</span><span class="sxs-lookup"><span data-stu-id="22bff-275">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="22bff-276">Tato přetížení nepoužívat názvy a jenom využívat výchozí nastavení z konfigurace.</span><span class="sxs-lookup"><span data-stu-id="22bff-276">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="22bff-277">*Změnit výchozí nastavení v kódu*</span><span class="sxs-lookup"><span data-stu-id="22bff-277">*Change the defaults in code*</span></span>

<span data-ttu-id="22bff-278">`ConfigureEndpointDefaults` a `ConfigureHttpsDefaults` lze změnit výchozí nastavení pro `ListenOptions` a `HttpsConnectionAdapterOptions`, včetně přepsání výchozí certifikát uvedený v předchozím scénáři.</span><span class="sxs-lookup"><span data-stu-id="22bff-278">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="22bff-279">`ConfigureEndpointDefaults` a `ConfigureHttpsDefaults` by měla být volána, než jsou nakonfigurovány žádné koncové body.</span><span class="sxs-lookup"><span data-stu-id="22bff-279">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="22bff-280">*Podpora kestrel SNI*</span><span class="sxs-lookup"><span data-stu-id="22bff-280">*Kestrel support for SNI*</span></span>

<span data-ttu-id="22bff-281">[Indikace názvu serveru (SNI)](https://tools.ietf.org/html/rfc6066#section-3) lze použít k hostování více domén na stejnou IP adresu a port.</span><span class="sxs-lookup"><span data-stu-id="22bff-281">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="22bff-282">Pro SNI funkce klient odešle název hostitele pro zabezpečené relace na server během metody handshake TLS, aby server můžete zadejte správný certifikát.</span><span class="sxs-lookup"><span data-stu-id="22bff-282">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="22bff-283">Klient použije certifikát zařízená šifrovanou komunikaci se serverem během zabezpečené relace, který následuje TLS handshake.</span><span class="sxs-lookup"><span data-stu-id="22bff-283">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="22bff-284">Kestrel podporuje SNI prostřednictvím `ServerCertificateSelector` zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="22bff-284">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="22bff-285">Zpětné volání je vyvolána jednou na připojení k povolit aplikaci zkontrolujte název hostitele a vyberte příslušný certifikát.</span><span class="sxs-lookup"><span data-stu-id="22bff-285">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="22bff-286">Podpora SNI vyžaduje:</span><span class="sxs-lookup"><span data-stu-id="22bff-286">SNI support requires:</span></span>

* <span data-ttu-id="22bff-287">Spuštěná na cílové rozhraní `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="22bff-287">Running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="22bff-288">Na `netcoreapp2.0` a `net461`, zpětné volání je voláno, ale `name` je vždy `null`.</span><span class="sxs-lookup"><span data-stu-id="22bff-288">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="22bff-289">`name` Je také `null` Pokud klient neposkytuje hostitele parametr name ve TLS handshake.</span><span class="sxs-lookup"><span data-stu-id="22bff-289">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="22bff-290">Všechny weby na stejnou instanci Kestrel spustit.</span><span class="sxs-lookup"><span data-stu-id="22bff-290">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="22bff-291">Kestrel nepodporuje sdílení adresu IP a portu ve více instancích bez reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="22bff-291">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

<span data-ttu-id="22bff-292">**Vytvoření vazby na soket TCP**</span><span class="sxs-lookup"><span data-stu-id="22bff-292">**Bind to a TCP socket**</span></span>

<span data-ttu-id="22bff-293">[Naslouchání](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) metoda váže na soket TCP a umožňuje lambda možností konfigurace certifikátu protokolu SSL:</span><span class="sxs-lookup"><span data-stu-id="22bff-293">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits SSL certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="22bff-294">Tento příklad konfiguruje SSL pro koncový bod s [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span><span class="sxs-lookup"><span data-stu-id="22bff-294">The example configures SSL for an endpoint with [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="22bff-295">Konfigurovat další nastavení Kestrel pro specifické koncové body pomocí stejné rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="22bff-295">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

<span data-ttu-id="22bff-296">**Vytvoření vazby na soket systému Unix**</span><span class="sxs-lookup"><span data-stu-id="22bff-296">**Bind to a Unix socket**</span></span>

<span data-ttu-id="22bff-297">Naslouchání soketu Unix s [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) pro zlepšení výkonu s Nginx, jak je uvedeno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="22bff-297">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="22bff-298">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="22bff-298">**Port 0**</span></span>

<span data-ttu-id="22bff-299">Když číslo portu `0` není zadaný, Kestrel dynamicky váže k dostupný port.</span><span class="sxs-lookup"><span data-stu-id="22bff-299">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="22bff-300">Následující příklad ukazuje, jak určit port, který Kestrel ve skutečnosti vázaný za běhu:</span><span class="sxs-lookup"><span data-stu-id="22bff-300">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="22bff-301">Když se aplikace spustí, určuje, výstup okna konzoly dynamický port, kde jsou dostupné aplikace:</span><span class="sxs-lookup"><span data-stu-id="22bff-301">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

<span data-ttu-id="22bff-302">**UseUrls, argument příkazového řádku – adresy URL, klíč konfigurace hostitele adresy URL a proměnných omezení ASPNETCORE_URLS prostředí**</span><span class="sxs-lookup"><span data-stu-id="22bff-302">**UseUrls, --urls command-line argument, urls host configuration key, and ASPNETCORE_URLS environment variable limitations**</span></span>

<span data-ttu-id="22bff-303">Nakonfigurujte koncové body pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="22bff-303">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="22bff-304">UseUrls</span><span class="sxs-lookup"><span data-stu-id="22bff-304">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="22bff-305">`--urls` Argument příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="22bff-305">`--urls` command-line argument</span></span>
* <span data-ttu-id="22bff-306">`urls` Klíč konfigurace hostitele</span><span class="sxs-lookup"><span data-stu-id="22bff-306">`urls` host configuration key</span></span>
* <span data-ttu-id="22bff-307">`ASPNETCORE_URLS` Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="22bff-307">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="22bff-308">Tyto metody jsou užitečné pro vytváření kódu spolupráci se servery než Kestrel.</span><span class="sxs-lookup"><span data-stu-id="22bff-308">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="22bff-309">Ale mějte na paměti tato omezení:</span><span class="sxs-lookup"><span data-stu-id="22bff-309">However, be aware of these limitations:</span></span>

* <span data-ttu-id="22bff-310">SSL nelze použít s těmito dvěma způsoby, pokud výchozí certifikát je součástí konfigurace koncového bodu protokolu HTTPS (například pomocí `KestrelServerOptions` konfigurace a konfigurační soubory, jak je znázorněno v tomto tématu výše).</span><span class="sxs-lookup"><span data-stu-id="22bff-310">SSL can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="22bff-311">Při jak `Listen` a `UseUrls` přístupy používají současně, `Listen` přepsat koncové body `UseUrls` koncové body.</span><span class="sxs-lookup"><span data-stu-id="22bff-311">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="22bff-312">**Konfigurace koncového bodu služby IIS**</span><span class="sxs-lookup"><span data-stu-id="22bff-312">**IIS endpoint configuration**</span></span>

<span data-ttu-id="22bff-313">Při použití IIS, vazby adresu URL pro službu IIS přepsat vazby jsou nastavené buď `Listen` nebo `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="22bff-313">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="22bff-314">Další informace najdete v tématu [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) tématu.</span><span class="sxs-lookup"><span data-stu-id="22bff-314">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22bff-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22bff-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="22bff-316">Ve výchozím nastavení, ASP.NET Core váže k `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="22bff-316">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="22bff-317">Konfigurace předpony adres URL a portů pro používání Kestrel:</span><span class="sxs-lookup"><span data-stu-id="22bff-317">Configure URL prefixes and ports for Kestrel using:</span></span>

* <span data-ttu-id="22bff-318">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) rozšiřující metoda</span><span class="sxs-lookup"><span data-stu-id="22bff-318">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) extension method</span></span>
* <span data-ttu-id="22bff-319">`--urls` Argument příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="22bff-319">`--urls` command-line argument</span></span>
* <span data-ttu-id="22bff-320">`urls` Klíč konfigurace hostitele</span><span class="sxs-lookup"><span data-stu-id="22bff-320">`urls` host configuration key</span></span>
* <span data-ttu-id="22bff-321">ASP.NET Core konfigurace systému, včetně `ASPNETCORE_URLS` proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="22bff-321">ASP.NET Core configuration system, including `ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="22bff-322">Další informace o těchto metodách v tématu [hostitelský](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="22bff-322">For more information on these methods, see [Hosting](xref:fundamentals/host/index).</span></span>

<span data-ttu-id="22bff-323">**Konfigurace koncového bodu služby IIS**</span><span class="sxs-lookup"><span data-stu-id="22bff-323">**IIS endpoint configuration**</span></span>

<span data-ttu-id="22bff-324">Při použití IIS, vazby adresu URL pro službu IIS přepsat vazby, která nastavuje `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="22bff-324">When using IIS, the URL bindings for IIS override bindings set by `UseUrls`.</span></span> <span data-ttu-id="22bff-325">Další informace najdete v tématu [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) tématu.</span><span class="sxs-lookup"><span data-stu-id="22bff-325">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

---

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a><span data-ttu-id="22bff-326">Konfigurace přenosu</span><span class="sxs-lookup"><span data-stu-id="22bff-326">Transport configuration</span></span>

<span data-ttu-id="22bff-327">Ve verzi ASP.NET Core 2.1 Kestrel na výchozí přenos je už podle Libuv ale na spravované sokety.</span><span class="sxs-lookup"><span data-stu-id="22bff-327">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="22bff-328">Jedná se o změnu ukončování řádků pro upgrade na 2.1, které volat aplikace ASP.NET 2.0 základní [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) a závisí na některý z následujících balíčků:</span><span class="sxs-lookup"><span data-stu-id="22bff-328">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) and depend on either of the following packages:</span></span>

* <span data-ttu-id="22bff-329">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (přímý odkaz na balíček)</span><span class="sxs-lookup"><span data-stu-id="22bff-329">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="22bff-330">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="22bff-330">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="22bff-331">Pro technologii ASP.NET Core 2.1 nebo novější projekty využívající [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) a vyžadují použití Libuv:</span><span class="sxs-lookup"><span data-stu-id="22bff-331">For ASP.NET Core 2.1 or later projects that use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and require the use of Libuv:</span></span>

* <span data-ttu-id="22bff-332">Přidat závislost [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) balíčku do souboru projektu aplikace:</span><span class="sxs-lookup"><span data-stu-id="22bff-332">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="2.1.0" />
    ```

* <span data-ttu-id="22bff-333">Volání [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span><span class="sxs-lookup"><span data-stu-id="22bff-333">Call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="22bff-334">Předpony adres URL</span><span class="sxs-lookup"><span data-stu-id="22bff-334">URL prefixes</span></span>

<span data-ttu-id="22bff-335">Při použití `UseUrls`, `--urls` argument příkazového řádku, `urls` klíč konfigurace hostitele, nebo `ASPNETCORE_URLS` proměnné prostředí, předpony adres URL může být v některém z následujících formátů.</span><span class="sxs-lookup"><span data-stu-id="22bff-335">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22bff-336">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="22bff-336">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="22bff-337">Platné jsou pouze předpony adres URL protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="22bff-337">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="22bff-338">Kestrel nepodporuje SSL při konfiguraci adresy URL vazby pomocí `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="22bff-338">Kestrel doesn't support SSL when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="22bff-339">Adresu IPv4 s číslo portu</span><span class="sxs-lookup"><span data-stu-id="22bff-339">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="22bff-340">`0.0.0.0` je zvláštní případ, která se sváže s všechny adresy IPv4.</span><span class="sxs-lookup"><span data-stu-id="22bff-340">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="22bff-341">Adresa IPv6 se číslo portu</span><span class="sxs-lookup"><span data-stu-id="22bff-341">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="22bff-342">`[::]` je ekvivalentem IPv6 IPv4 `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="22bff-342">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="22bff-343">Název hostitele s číslem portu</span><span class="sxs-lookup"><span data-stu-id="22bff-343">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="22bff-344">Názvy hostitelů `*`, a `+`, nejsou speciální.</span><span class="sxs-lookup"><span data-stu-id="22bff-344">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="22bff-345">Nic nebyl rozpoznán jako platná IP adresa nebo `localhost` váže pro všechny IP adresy IPv6 a IPv4.</span><span class="sxs-lookup"><span data-stu-id="22bff-345">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="22bff-346">Chcete-li vytvořit vazbu různé názvy hostitelů na jiné aplikace ASP.NET Core na stejný port, použijte [HTTP.sys](xref:fundamentals/servers/httpsys) nebo reverzní proxy server, jako jsou služby IIS, Nginx nebo Apache.</span><span class="sxs-lookup"><span data-stu-id="22bff-346">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="22bff-347">Pokud nepoužíváte reverzní proxy server s hostitelem filtrování povoleno, povolit [hostitele filtrování](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="22bff-347">If not using a reverse proxy with host filtering enabled, enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="22bff-348">Hostitele `localhost` název s číslo nebo zpětné smyčky IP port s číslem portu</span><span class="sxs-lookup"><span data-stu-id="22bff-348">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="22bff-349">Když `localhost` není zadaný, Kestrel se pokusí vytvořit vazbu na rozhraní zpětné smyčky protokolu IPv4 a IPv6.</span><span class="sxs-lookup"><span data-stu-id="22bff-349">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="22bff-350">Pokud požadovaný port je používán jinou službou buď rozhraní zpětné smyčky, Kestrel se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="22bff-350">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="22bff-351">Pokud buď rozhraní zpětné smyčky je k dispozici z jiného důvodu (Většina běžně, protože protokol IPv6 není podporován), Kestrel protokoly upozornění.</span><span class="sxs-lookup"><span data-stu-id="22bff-351">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22bff-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22bff-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="22bff-353">Adresu IPv4 s číslo portu</span><span class="sxs-lookup"><span data-stu-id="22bff-353">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="22bff-354">`0.0.0.0` je zvláštní případ, která se sváže s všechny adresy IPv4.</span><span class="sxs-lookup"><span data-stu-id="22bff-354">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="22bff-355">Adresa IPv6 se číslo portu</span><span class="sxs-lookup"><span data-stu-id="22bff-355">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  <span data-ttu-id="22bff-356">`[::]` je ekvivalentem IPv6 IPv4 `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="22bff-356">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="22bff-357">Název hostitele s číslem portu</span><span class="sxs-lookup"><span data-stu-id="22bff-357">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="22bff-358">Názvy hostitelů `*`, a `+` nejsou speciální.</span><span class="sxs-lookup"><span data-stu-id="22bff-358">Host names, `*`, and `+` aren't special.</span></span> <span data-ttu-id="22bff-359">Všechno, co není rozpoznaný IP adresu nebo `localhost` váže pro všechny IP adresy IPv6 a IPv4.</span><span class="sxs-lookup"><span data-stu-id="22bff-359">Anything that isn't a recognized IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="22bff-360">Chcete-li vytvořit vazbu různé názvy hostitelů na jiné aplikace ASP.NET Core na stejný port, použijte [WebListener](xref:fundamentals/servers/weblistener) nebo reverzní proxy server, jako jsou služby IIS, Nginx nebo Apache.</span><span class="sxs-lookup"><span data-stu-id="22bff-360">To bind different host names to different ASP.NET Core apps on the same port, use [WebListener](xref:fundamentals/servers/weblistener) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="22bff-361">Hostitele `localhost` název s číslo nebo zpětné smyčky IP port s číslem portu</span><span class="sxs-lookup"><span data-stu-id="22bff-361">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="22bff-362">Když `localhost` není zadaný, Kestrel se pokusí vytvořit vazbu na rozhraní zpětné smyčky protokolu IPv4 a IPv6.</span><span class="sxs-lookup"><span data-stu-id="22bff-362">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="22bff-363">Pokud požadovaný port je používán jinou službou buď rozhraní zpětné smyčky, Kestrel se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="22bff-363">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="22bff-364">Pokud buď rozhraní zpětné smyčky je k dispozici z jiného důvodu (Většina běžně, protože protokol IPv6 není podporován), Kestrel protokoly upozornění.</span><span class="sxs-lookup"><span data-stu-id="22bff-364">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

* <span data-ttu-id="22bff-365">UNIX soketů</span><span class="sxs-lookup"><span data-stu-id="22bff-365">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock
  ```

<span data-ttu-id="22bff-366">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="22bff-366">**Port 0**</span></span>

<span data-ttu-id="22bff-367">Pokud je číslo portu `0` není zadaný, Kestrel dynamicky váže k dostupný port.</span><span class="sxs-lookup"><span data-stu-id="22bff-367">When the port number is `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="22bff-368">Vytvoření vazby na portu `0` povolené pro všechny název hostitele nebo IP adresu s výjimkou pro `localhost`.</span><span class="sxs-lookup"><span data-stu-id="22bff-368">Binding to port `0` is allowed for any host name or IP except for `localhost`.</span></span>

<span data-ttu-id="22bff-369">Když se aplikace spustí, určuje, výstup okna konzoly dynamický port, kde jsou dostupné aplikace:</span><span class="sxs-lookup"><span data-stu-id="22bff-369">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="22bff-370">**Předpony adres URL pro protokol SSL**</span><span class="sxs-lookup"><span data-stu-id="22bff-370">**URL prefixes for SSL**</span></span>

<span data-ttu-id="22bff-371">Pokud volání `UseHttps` metoda rozšíření, nezapomeňte zahrnout předpony adres URL s `https:`:</span><span class="sxs-lookup"><span data-stu-id="22bff-371">If calling the `UseHttps` extension method, be sure to include URL prefixes with `https:`:</span></span>

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
> <span data-ttu-id="22bff-372">Protokol HTTPS a HTTP nemůže být hostovaná na stejném portu.</span><span class="sxs-lookup"><span data-stu-id="22bff-372">HTTPS and HTTP can't be hosted on the same port.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

---

## <a name="host-filtering"></a><span data-ttu-id="22bff-373">Filtrování hostitele</span><span class="sxs-lookup"><span data-stu-id="22bff-373">Host filtering</span></span>

<span data-ttu-id="22bff-374">I když Kestrel podporuje konfigurace, například podle předpony `http://example.com:5000`, Kestrel z velké části ignoruje název hostitele.</span><span class="sxs-lookup"><span data-stu-id="22bff-374">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="22bff-375">Hostitele `localhost` je zvláštní případ použité pro vazbu na adresy zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="22bff-375">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="22bff-376">Žádný hostitel, jiné než explicitní IP adresu se váže k všechny veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="22bff-376">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="22bff-377">Žádná z těchto informací se používá k ověření požadavku `Host` hlavičky.</span><span class="sxs-lookup"><span data-stu-id="22bff-377">None of this information is used to validate request `Host` headers.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="22bff-378">Jako alternativní řešení hostování za reverzní proxy server s filtrování hlavičky hostitele.</span><span class="sxs-lookup"><span data-stu-id="22bff-378">As a workaround, host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="22bff-379">Toto je jediný podporovaný scénář pro Kestrel v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="22bff-379">This is the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="22bff-380">Jako alternativní řešení, použít pro filtrování požadavků podle middlewaru `Host` hlavičky:</span><span class="sxs-lookup"><span data-stu-id="22bff-380">As a workaround, use middleware to filter requests by the `Host` header:</span></span>

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

<span data-ttu-id="22bff-381">Zaregistrovat předchozím `HostFilteringMiddleware` v `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="22bff-381">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="22bff-382">Všimněte si, že [řazení middleware registrace](xref:fundamentals/middleware/index#ordering) je důležité.</span><span class="sxs-lookup"><span data-stu-id="22bff-382">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#ordering) is important.</span></span> <span data-ttu-id="22bff-383">K registraci by mělo dojít okamžitě po registraci diagnostiky Middleware (například `app.UseExceptionHandler`).</span><span class="sxs-lookup"><span data-stu-id="22bff-383">Registration should occur immediately after Diagnostic Middleware registration (for example, `app.UseExceptionHandler`).</span></span>

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

<span data-ttu-id="22bff-384">Middleware očekává `AllowedHosts` klíče v *appSettings.JSON určený*/*appsettings.\< EnvironmentName > .json*.</span><span class="sxs-lookup"><span data-stu-id="22bff-384">The middleware expects an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="22bff-385">Hodnota je oddělený středníkem seznam názvů hostitelů bez čísla portů:</span><span class="sxs-lookup"><span data-stu-id="22bff-385">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="22bff-386">Jako alternativní řešení použijte filtrování Middleware hostitele.</span><span class="sxs-lookup"><span data-stu-id="22bff-386">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="22bff-387">Poskytuje Middleware filtrování hostitele [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) balíček, který je součástí [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="22bff-387">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="22bff-388">Middleware přidává [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), který volá [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span><span class="sxs-lookup"><span data-stu-id="22bff-388">The middleware is added by [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span></span>

<span data-ttu-id="22bff-389">[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="22bff-389">[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]</span></span>

<span data-ttu-id="22bff-390">Ve výchozím nastavení vypnutá Middleware filtrování hostitele.</span><span class="sxs-lookup"><span data-stu-id="22bff-390">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="22bff-391">Chcete-li povolit middleware, definovat `AllowedHosts` klíče v *appSettings.JSON určený*/*appsettings.\< EnvironmentName > .json*.</span><span class="sxs-lookup"><span data-stu-id="22bff-391">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="22bff-392">Hodnota je oddělený středníkem seznam názvů hostitelů bez čísla portů:</span><span class="sxs-lookup"><span data-stu-id="22bff-392">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="22bff-393">*appSettings.JSON určený*:</span><span class="sxs-lookup"><span data-stu-id="22bff-393">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="22bff-394">[Předané hlavičky Middleware](xref:host-and-deploy/proxy-load-balancer) má také [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) možnost.</span><span class="sxs-lookup"><span data-stu-id="22bff-394">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) option.</span></span> <span data-ttu-id="22bff-395">Přesměrovaná Middleware hlavičky a hostitele filtrování Middleware mají podobné funkce pro různé scénáře.</span><span class="sxs-lookup"><span data-stu-id="22bff-395">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="22bff-396">Nastavení `AllowedHosts` se předají Middleware hlavičky je vhodné, pokud Hlavička hostitele není zachován při předávání požadavků s reverzní proxy server nebo službu Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="22bff-396">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the Host header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="22bff-397">Nastavení `AllowedHosts` s hostiteli filtrování middlewaru je vhodné při Kestrel slouží jako hraniční server nebo pokud je hlavička hostitele přímo předávat.</span><span class="sxs-lookup"><span data-stu-id="22bff-397">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as an edge server or when the Host header is directly forwarded.</span></span>
>
> <span data-ttu-id="22bff-398">Další informace o předávaných Middleware hlavičky, najdete v části [konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="22bff-398">For more information on Forwarded Headers Middleware, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="22bff-399">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="22bff-399">Additional resources</span></span>

* [<span data-ttu-id="22bff-400">Vynucení protokolu HTTPS</span><span class="sxs-lookup"><span data-stu-id="22bff-400">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="22bff-401">Kestrel zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="22bff-401">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
* [<span data-ttu-id="22bff-402">RFC 7230: Syntaxe a směrování zpráv (část 5.4: hostitele)</span><span class="sxs-lookup"><span data-stu-id="22bff-402">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
* [<span data-ttu-id="22bff-403">Konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="22bff-403">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
