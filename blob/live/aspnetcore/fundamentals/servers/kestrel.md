---
title: "Kestrel webového serveru implementace v ASP.NET Core"
author: tdykstra
description: "Představuje Kestrel, a platformy webového serveru pro ASP.NET Core podle libuv."
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3695a6a127f77bd90538d72af6112ccf507f3482
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="3965c-103">Úvod k implementaci serveru webové Kestrel v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3965c-103">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="3965c-104">Podle [tní Dykstra](https://github.com/tdykstra), [Jan Ross](https://github.com/Tratcher), a [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="3965c-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="3965c-105">Kestrel je napříč platformami [webového serveru pro ASP.NET Core](index.md) na základě [libuv](https://github.com/libuv/libuv), knihovny a platformy asynchronní vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="3965c-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="3965c-106">Kestrel je webový server, který je zahrnut ve výchozím nastavení v šablony projektů ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3965c-106">Kestrel is the web server that is included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="3965c-107">Kestrel podporuje následující funkce:</span><span class="sxs-lookup"><span data-stu-id="3965c-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="3965c-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="3965c-108">HTTPS</span></span>
  * <span data-ttu-id="3965c-109">Neprůhledné upgrade používaným pro povolení [objekty WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="3965c-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="3965c-110">Sokety UNIX pro vysoký výkon za Nginx</span><span class="sxs-lookup"><span data-stu-id="3965c-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="3965c-111">Kestrel je podporována na všech platformách a verze, které podporuje .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3965c-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3965c-112">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3965c-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3965c-113">[Zobrazit nebo stáhnout ukázkový kód pro 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3965c-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3965c-114">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="3965c-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3965c-115">[Zobrazit nebo stáhnout ukázkový kód pro 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3965c-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="3965c-116">Kdy použít Kestrel s reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="3965c-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3965c-117">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3965c-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3965c-118">Kestrel můžete použít samostatně nebo se *reverzní proxy server*, jako jsou například služby IIS, Nginx nebo Apache.</span><span class="sxs-lookup"><span data-stu-id="3965c-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="3965c-119">Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování.</span><span class="sxs-lookup"><span data-stu-id="3965c-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel komunikuje přímo s Internetu bez reverzní proxy server](kestrel/_static/kestrel-to-internet2.png)

![Kestrel nepřímo komunikuje v Internetu přes reverzní proxy server, například služby IIS, Nginx nebo Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="3965c-122">Buď konfiguraci &mdash; s nebo bez reverzní proxy server &mdash; lze také použít, pokud Kestrel má přístup pouze k interní síti.</span><span class="sxs-lookup"><span data-stu-id="3965c-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3965c-123">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="3965c-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3965c-124">Pokud vaše aplikace přijímá požadavky jenom z interní sítě, můžete použít Kestrel samostatně.</span><span class="sxs-lookup"><span data-stu-id="3965c-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel komunikuje přímo s interní sítě](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="3965c-126">Pokud jste vystavit aplikace k Internetu, musí používat službu IIS, Nginx nebo Apache jako *reverzní proxy server*.</span><span class="sxs-lookup"><span data-stu-id="3965c-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="3965c-127">Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování.</span><span class="sxs-lookup"><span data-stu-id="3965c-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel nepřímo komunikuje v Internetu přes reverzní proxy server, například služby IIS, Nginx nebo Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="3965c-129">Reverzní proxy server je vyžadována pro nasazení okraj (vystavený pro přenosy z Internetu) z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="3965c-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="3965c-130">Verze 1.x Kestrel nemají úplný doplněk obranu proti útokům.</span><span class="sxs-lookup"><span data-stu-id="3965c-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="3965c-131">To zahrnuje, avšak není omezeno na příslušné vypršení časových limitů, omezení velikosti a omezení počtu souběžných připojení.</span><span class="sxs-lookup"><span data-stu-id="3965c-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="3965c-132">Scénáře, který vyžaduje reverzní proxy server je, když máte více aplikací, které sdílejí stejnou adresu IP a portu spouští na jednom serveru.</span><span class="sxs-lookup"><span data-stu-id="3965c-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="3965c-133">To není možné s Kestrel přímo Kestrel, který nepodporuje sdílení stejné IP adresy a portu mezi více procesy.</span><span class="sxs-lookup"><span data-stu-id="3965c-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="3965c-134">Když konfigurujete Kestrel naslouchat na portu, zpracovává všechny přenosy pro tento port bez ohledu na hlavičku hostitele.</span><span class="sxs-lookup"><span data-stu-id="3965c-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="3965c-135">Reverzní proxy server, můžete sdílet porty musí předat pak Kestrel na jedinečné IP adresy a portu.</span><span class="sxs-lookup"><span data-stu-id="3965c-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="3965c-136">I když není požadovaných reverzní proxy server, pomocí jednoho může být vhodný z jiných důvodů:</span><span class="sxs-lookup"><span data-stu-id="3965c-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="3965c-137">Ho můžete omezit vystavených útoku.</span><span class="sxs-lookup"><span data-stu-id="3965c-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="3965c-138">Poskytuje další úroveň volitelné konfigurace a obrany.</span><span class="sxs-lookup"><span data-stu-id="3965c-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="3965c-139">Může integrovat lépe stávající infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="3965c-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="3965c-140">Usnadňují Vyrovnávání zatížení a nastavení protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="3965c-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="3965c-141">Pouze reverzní proxy server vyžaduje certifikát SSL a tento server může komunikovat s vašimi aplikací servery v interní síti pomocí prostý protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3965c-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="3965c-142">Jak používat Kestrel v aplikacích ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3965c-142">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3965c-143">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3965c-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3965c-144">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) je součástí balíčku [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="3965c-144">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="3965c-145">Šablony projektů ASP.NET Core pomocí Kestrel ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="3965c-145">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="3965c-146">V *Program.cs*, kód zavolá metodu šablony `CreateDefaultBuilder`, který volá [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) na pozadí.</span><span class="sxs-lookup"><span data-stu-id="3965c-146">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="3965c-147">Pokud potřebujete nakonfigurovat možnosti Kestrel, zavolejte `UseKestrel` v *Program.cs* jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="3965c-147">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3965c-148">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="3965c-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3965c-149">Nainstalujte [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="3965c-149">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="3965c-150">Volání [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) rozšiřující metody na `WebHostBuilder` v vaše `Main` metoda, zadání žádné [Kestrel možnosti](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) , které potřebujete, jak je znázorněno v následujícím oddílu.</span><span class="sxs-lookup"><span data-stu-id="3965c-150">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="3965c-151">Možnosti kestrel</span><span class="sxs-lookup"><span data-stu-id="3965c-151">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3965c-152">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3965c-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3965c-153">Kestrel webového serveru obsahuje možnosti konfigurace omezení, které jsou užitečné zejména v internetových nasazení.</span><span class="sxs-lookup"><span data-stu-id="3965c-153">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="3965c-154">Zde naleznete některá omezení, které můžete zadat:</span><span class="sxs-lookup"><span data-stu-id="3965c-154">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="3965c-155">Maximální počet klientských připojení</span><span class="sxs-lookup"><span data-stu-id="3965c-155">Maximum client connections</span></span>
- <span data-ttu-id="3965c-156">Velikost textu maximální požadavku</span><span class="sxs-lookup"><span data-stu-id="3965c-156">Maximum request body size</span></span>
- <span data-ttu-id="3965c-157">Minimální požadavek textu přenosová rychlost</span><span class="sxs-lookup"><span data-stu-id="3965c-157">Minimum request body data rate</span></span>

<span data-ttu-id="3965c-158">Nastavení těchto omezení a ostatní uživatele v `Limits` vlastnost [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) třídy.</span><span class="sxs-lookup"><span data-stu-id="3965c-158">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="3965c-159">`Limits` Vlastnost obsahuje instanci [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) třídy.</span><span class="sxs-lookup"><span data-stu-id="3965c-159">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="3965c-160">**Maximální počet klientských připojení**</span><span class="sxs-lookup"><span data-stu-id="3965c-160">**Maximum client connections**</span></span>

<span data-ttu-id="3965c-161">Maximální počet souběžných otevřete připojení TCP lze nastavit pro celou aplikaci s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="3965c-161">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="3965c-162">Je samostatný limit pro připojení, které byly upgradované z protokolu HTTP nebo HTTPS na jiný protokol (například pro objekty WebSockets požadavek).</span><span class="sxs-lookup"><span data-stu-id="3965c-162">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="3965c-163">Po upgradu připojení není započítané `MaxConcurrentConnections` limit.</span><span class="sxs-lookup"><span data-stu-id="3965c-163">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="3965c-164">Maximální počet připojení je neomezená (null) ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="3965c-164">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="3965c-165">**Velikost textu maximální požadavku**</span><span class="sxs-lookup"><span data-stu-id="3965c-165">**Maximum request body size**</span></span>

<span data-ttu-id="3965c-166">Výchozí velikost těla maximální požadavku je 30,000,000 bajtů, což je přibližně 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="3965c-166">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="3965c-167">Doporučený způsob přepsat omezení v aplikaci ASP.NET MVC základní se má používat [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atribut na metodu akce:</span><span class="sxs-lookup"><span data-stu-id="3965c-167">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="3965c-168">Tady je příklad, který ukazuje, jak nakonfigurovat omezení pro celou aplikaci, každou žádost:</span><span class="sxs-lookup"><span data-stu-id="3965c-168">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="3965c-169">Můžete přepsat nastavení konkrétního požadavku v middlewaru.:</span><span class="sxs-lookup"><span data-stu-id="3965c-169">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="3965c-170">Pokud se pokusíte nakonfigurovat limit na požadavek po spuštění aplikace čtení požadavku, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="3965c-170">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="3965c-171">Je `IsReadOnly` vlastnost, která se dozvíte, pokud `MaxRequestBodySize` vlastnost je ve stavu jen pro čtení, tzn. je příliš pozdě Konfigurace limitu.</span><span class="sxs-lookup"><span data-stu-id="3965c-171">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="3965c-172">**Minimální požadavek textu přenosová rychlost**</span><span class="sxs-lookup"><span data-stu-id="3965c-172">**Minimum request body data rate**</span></span>

<span data-ttu-id="3965c-173">Kestrel kontroluje každou sekundu v případě pochází data na určenou míru v bajtech za sekundu.</span><span class="sxs-lookup"><span data-stu-id="3965c-173">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="3965c-174">Pokud rychlost klesne pod minimální, vypršení časového limitu připojení. Poskytnutá lhůta je množství času, aby Kestrel dává klienta ke zvýšení jeho rychlost odesílání až minimální; rychlost, jakou kontrolována během této doby.</span><span class="sxs-lookup"><span data-stu-id="3965c-174">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate is not checked during that time.</span></span> <span data-ttu-id="3965c-175">Období odkladu pomáhá zabránit, vyřazení připojení, které jsou původně odesílání dat s nízkou rychlostí z důvodu zpomalit TCP-start.</span><span class="sxs-lookup"><span data-stu-id="3965c-175">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="3965c-176">Výchozí minimální rychlost je 240 bajtů za sekundu, období odkladu 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="3965c-176">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="3965c-177">Minimální rychlost platí také pro odpověď.</span><span class="sxs-lookup"><span data-stu-id="3965c-177">A minimum rate also applies to the response.</span></span> <span data-ttu-id="3965c-178">Kód pro nastavení limitu požadavků a omezení odpovědi je stejný kromě nutnosti `RequestBody` nebo `Response` v názvech vlastností a rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3965c-178">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="3965c-179">Tady je příklad, který ukazuje, jak konfigurovat minimální datové sazby v *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="3965c-179">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="3965c-180">Sazby za žádosti můžete nakonfigurovat v middlewaru:</span><span class="sxs-lookup"><span data-stu-id="3965c-180">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="3965c-181">Informace o dalších možnostech Kestrel najdete v tématu následující třídy:</span><span class="sxs-lookup"><span data-stu-id="3965c-181">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="3965c-182">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="3965c-182">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="3965c-183">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="3965c-183">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="3965c-184">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="3965c-184">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3965c-185">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="3965c-185">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3965c-186">Informace o možnostech Kestrel najdete v tématu [KestrelServerOptions třída](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="3965c-186">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="3965c-187">Konfigurace koncového bodu</span><span class="sxs-lookup"><span data-stu-id="3965c-187">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3965c-188">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3965c-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3965c-189">Ve výchozím nastavení ASP.NET Core váže k `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="3965c-189">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="3965c-190">Konfigurace předpony adres URL a portů pro Kestrel tak, aby naslouchala na voláním `Listen` nebo `ListenUnixSocket` metody na `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="3965c-190">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="3965c-191">(`UseUrls`, `urls` argument příkazového řádku a proměnnou prostředí ASPNETCORE_URLS také pracovní ale mají omezení uvedeno [dále v tomto článku](#useurls-limitations).)</span><span class="sxs-lookup"><span data-stu-id="3965c-191">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="3965c-192">**Vytvoření vazby na soket TCP**</span><span class="sxs-lookup"><span data-stu-id="3965c-192">**Bind to a TCP socket**</span></span>

<span data-ttu-id="3965c-193">`Listen` Metoda váže na soket TCP a lambda možnosti vám umožní nakonfigurovat certifikát SSL:</span><span class="sxs-lookup"><span data-stu-id="3965c-193">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="3965c-194">Všimněte si, jak tento příklad konfiguruje SSL pro koncový bod konkrétní pomocí [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="3965c-194">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="3965c-195">Konfigurovat další nastavení Kestrel pro konkrétní koncové body, můžete použít stejné rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3965c-195">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="3965c-196">**Vytvoření vazby na soket systému Unix**</span><span class="sxs-lookup"><span data-stu-id="3965c-196">**Bind to a Unix socket**</span></span>

<span data-ttu-id="3965c-197">Můžete naslouchání pro zlepšení výkonu s Nginx, soketu Unix, jak je uvedeno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="3965c-197">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="3965c-198">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="3965c-198">**Port 0**</span></span>

<span data-ttu-id="3965c-199">Pokud zadáte číslo portu 0, Kestrel dynamicky sváže dostupný port.</span><span class="sxs-lookup"><span data-stu-id="3965c-199">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="3965c-200">Následující příklad ukazuje, jak určit port, který Kestrel ve skutečnosti vázaný za běhu:</span><span class="sxs-lookup"><span data-stu-id="3965c-200">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="3965c-201">**Omezení UseUrls**</span><span class="sxs-lookup"><span data-stu-id="3965c-201">**UseUrls limitations**</span></span>

<span data-ttu-id="3965c-202">Koncové body můžete nakonfigurovat pomocí volání `UseUrls` metoda nebo pomocí `urls` argument příkazového řádku nebo proměnná prostředí ASPNETCORE_URLS.</span><span class="sxs-lookup"><span data-stu-id="3965c-202">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="3965c-203">Tyto metody jsou užitečné, pokud chcete, aby váš kód pro práci se servery než Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3965c-203">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="3965c-204">Ale mějte na paměti tato omezení:</span><span class="sxs-lookup"><span data-stu-id="3965c-204">However, be aware of these limitations:</span></span>

* <span data-ttu-id="3965c-205">Pomocí těchto metod, nelze používat protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="3965c-205">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="3965c-206">Pokud používáte obě `Listen` metoda a `UseUrls`, `Listen` přepsat koncové body `UseUrls` koncové body.</span><span class="sxs-lookup"><span data-stu-id="3965c-206">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="3965c-207">**Konfigurace koncového bodu pro službu IIS**</span><span class="sxs-lookup"><span data-stu-id="3965c-207">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="3965c-208">Pokud používáte službu IIS, vazby adresu URL pro službu IIS přepsat žádné vazby, která jste nastavili voláním buď `Listen` nebo `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3965c-208">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="3965c-209">Další informace najdete v tématu [Úvod k modulu jádra ASP.NET](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="3965c-209">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3965c-210">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="3965c-210">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3965c-211">Ve výchozím nastavení ASP.NET Core váže k `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="3965c-211">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="3965c-212">Můžete nakonfigurovat předpony adres URL a portů pro Kestrel tak, aby naslouchala na pomocí `UseUrls` metoda rozšíření `urls` argument příkazového řádku nebo konfigurační systém ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3965c-212">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="3965c-213">Další informace o těchto metodách v tématu [hostitelský](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="3965c-213">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="3965c-214">Informace o fungování vazby adresy URL při použití IIS jako reverzní proxy server, najdete v článku [ASP.NET Core modulu](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="3965c-214">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="3965c-215">Předpony adres URL</span><span class="sxs-lookup"><span data-stu-id="3965c-215">URL prefixes</span></span>

<span data-ttu-id="3965c-216">Když zavoláte `UseUrls` nebo použít `urls` argument příkazového řádku nebo proměnná prostředí ASPNETCORE_URLS předpony adres URL může být v některém z následujících formátů.</span><span class="sxs-lookup"><span data-stu-id="3965c-216">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3965c-217">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3965c-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3965c-218">Pouze předpony adres URL protokolu HTTP jsou platné; Kestrel SSL nepodporuje, když konfigurujete vazby adresu URL pomocí `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3965c-218">Only HTTP URL prefixes are valid; Kestrel does not support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="3965c-219">Adresu IPv4 s číslo portu</span><span class="sxs-lookup"><span data-stu-id="3965c-219">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="3965c-220">0.0.0.0 je zvláštní případ, která se sváže s všechny adresy IPv4.</span><span class="sxs-lookup"><span data-stu-id="3965c-220">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="3965c-221">Adresa IPv6 se číslo portu</span><span class="sxs-lookup"><span data-stu-id="3965c-221">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="3965c-222">[:] je ekvivalentem IPv6 IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="3965c-222">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="3965c-223">Název hostitele s číslem portu</span><span class="sxs-lookup"><span data-stu-id="3965c-223">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="3965c-224">Názvy hostitelů \*, a + nejsou speciální.</span><span class="sxs-lookup"><span data-stu-id="3965c-224">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="3965c-225">Všechno, co není rozpoznaný IP adresu nebo "localhost" vytvoří vazbu pro všechny IP adresy IPv6 a IPv4.</span><span class="sxs-lookup"><span data-stu-id="3965c-225">Anything that is not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="3965c-226">Pokud potřebujete vytvořit vazbu různé názvy hostitelů na různé aplikace ASP.NET Core na stejném portu, použijte [HTTP.sys](httpsys.md) nebo reverzní proxy server, jako jsou služby IIS, Nginx nebo Apache.</span><span class="sxs-lookup"><span data-stu-id="3965c-226">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="3965c-227">Název "Localhost" s IP číslo nebo zpětné smyčky port s číslem portu</span><span class="sxs-lookup"><span data-stu-id="3965c-227">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="3965c-228">Když `localhost` není zadaný, Kestrel se pokusí vytvořit vazbu na rozhraní zpětné smyčky protokolu IPv4 a IPv6.</span><span class="sxs-lookup"><span data-stu-id="3965c-228">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="3965c-229">Pokud požadovaný port je používán jinou službou buď rozhraní zpětné smyčky, Kestrel se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="3965c-229">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="3965c-230">Pokud buď rozhraní zpětné smyčky je k dispozici z jiného důvodu (Většina běžně, protože protokol IPv6 není podporován), Kestrel protokoly upozornění.</span><span class="sxs-lookup"><span data-stu-id="3965c-230">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3965c-231">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="3965c-231">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="3965c-232">Adresu IPv4 s číslo portu</span><span class="sxs-lookup"><span data-stu-id="3965c-232">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="3965c-233">0.0.0.0 je zvláštní případ, která se sváže s všechny adresy IPv4.</span><span class="sxs-lookup"><span data-stu-id="3965c-233">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="3965c-234">Adresa IPv6 se číslo portu</span><span class="sxs-lookup"><span data-stu-id="3965c-234">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="3965c-235">[:] je ekvivalentem IPv6 IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="3965c-235">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="3965c-236">Název hostitele s číslem portu</span><span class="sxs-lookup"><span data-stu-id="3965c-236">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="3965c-237">Názvy hostitelů \*, a + nejsou speciální.</span><span class="sxs-lookup"><span data-stu-id="3965c-237">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="3965c-238">Váže všechno, co není rozpoznaný IP adresu nebo "localhost" pro všechny IP adresy IPv6 a IPv4.</span><span class="sxs-lookup"><span data-stu-id="3965c-238">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="3965c-239">Pokud potřebujete vytvořit vazbu různé názvy hostitelů na různé aplikace ASP.NET Core na stejném portu, použijte [WebListener](weblistener.md) nebo reverzní proxy server, jako jsou služby IIS, Nginx nebo Apache.</span><span class="sxs-lookup"><span data-stu-id="3965c-239">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="3965c-240">Název "Localhost" s IP číslo nebo zpětné smyčky port s číslem portu</span><span class="sxs-lookup"><span data-stu-id="3965c-240">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="3965c-241">Když `localhost` není zadaný, Kestrel se pokusí vytvořit vazbu na rozhraní zpětné smyčky protokolu IPv4 a IPv6.</span><span class="sxs-lookup"><span data-stu-id="3965c-241">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="3965c-242">Pokud požadovaný port je používán jinou službou buď rozhraní zpětné smyčky, Kestrel se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="3965c-242">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="3965c-243">Pokud buď rozhraní zpětné smyčky je k dispozici z jiného důvodu (Většina běžně, protože protokol IPv6 není podporován), Kestrel protokoly upozornění.</span><span class="sxs-lookup"><span data-stu-id="3965c-243">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="3965c-244">UNIX soketů</span><span class="sxs-lookup"><span data-stu-id="3965c-244">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="3965c-245">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="3965c-245">**Port 0**</span></span>

<span data-ttu-id="3965c-246">Pokud zadáte číslo portu 0, Kestrel dynamicky sváže dostupný port.</span><span class="sxs-lookup"><span data-stu-id="3965c-246">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="3965c-247">Vytvoření vazby na portu 0 je povoleno pro všechny název hostitele nebo IP adresu s výjimkou `localhost` název.</span><span class="sxs-lookup"><span data-stu-id="3965c-247">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="3965c-248">Následující příklad ukazuje, jak určit port, který Kestrel ve skutečnosti vázaný za běhu:</span><span class="sxs-lookup"><span data-stu-id="3965c-248">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="3965c-249">**Předpony adres URL pro protokol SSL**</span><span class="sxs-lookup"><span data-stu-id="3965c-249">**URL prefixes for SSL**</span></span>

<span data-ttu-id="3965c-250">Nezapomeňte zahrnout předpony adres URL s `https:` při volání `UseHttps` rozšíření metoda, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="3965c-250">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

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
> <span data-ttu-id="3965c-251">Protokol HTTPS a HTTP nemůže být hostovaná na stejném portu.</span><span class="sxs-lookup"><span data-stu-id="3965c-251">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="3965c-252">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3965c-252">Next steps</span></span>

<span data-ttu-id="3965c-253">Další informace naleznete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="3965c-253">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3965c-254">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3965c-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="3965c-255">Ukázková aplikace pro 2.x</span><span class="sxs-lookup"><span data-stu-id="3965c-255">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="3965c-256">Kestrel zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="3965c-256">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3965c-257">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="3965c-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="3965c-258">Ukázková aplikace pro 1.x</span><span class="sxs-lookup"><span data-stu-id="3965c-258">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="3965c-259">Kestrel zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="3965c-259">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
