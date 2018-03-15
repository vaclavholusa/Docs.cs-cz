---
title: "Kestrel webového serveru implementace v ASP.NET Core"
author: tdykstra
description: "Představuje Kestrel, a platformy webového serveru pro ASP.NET Core podle libuv."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: be465c9e8803e4d348cdd14181b4ea147f75e1a0
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="b914b-103">Kestrel webového serveru implementace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b914b-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="b914b-104">Podle [tní Dykstra](https://github.com/tdykstra), [Jan Ross](https://github.com/Tratcher), a [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="b914b-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="b914b-105">Kestrel je napříč platformami [webového serveru pro ASP.NET Core](index.md) na základě [libuv](https://github.com/libuv/libuv), knihovny a platformy asynchronní vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="b914b-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="b914b-106">Kestrel je webový server, který je zahrnut ve výchozím nastavení v šablony projektů ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b914b-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="b914b-107">Kestrel podporuje následující funkce:</span><span class="sxs-lookup"><span data-stu-id="b914b-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="b914b-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="b914b-108">HTTPS</span></span>
  * <span data-ttu-id="b914b-109">Neprůhledné upgrade používaným pro povolení [objekty WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="b914b-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="b914b-110">Sokety UNIX pro vysoký výkon za Nginx</span><span class="sxs-lookup"><span data-stu-id="b914b-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="b914b-111">Kestrel je podporována na všech platformách a verze, které podporuje .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b914b-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b914b-112">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="b914b-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b914b-113">[Zobrazit nebo stáhnout ukázkový kód pro 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b914b-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b914b-114">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="b914b-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b914b-115">[Zobrazit nebo stáhnout ukázkový kód pro 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b914b-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="b914b-116">Kdy použít Kestrel s reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="b914b-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b914b-117">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="b914b-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b914b-118">Kestrel můžete použít samostatně nebo se *reverzní proxy server*, jako jsou například služby IIS, Nginx nebo Apache.</span><span class="sxs-lookup"><span data-stu-id="b914b-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="b914b-119">Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování.</span><span class="sxs-lookup"><span data-stu-id="b914b-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel komunikuje přímo s Internetu bez reverzní proxy server](kestrel/_static/kestrel-to-internet2.png)

![Kestrel nepřímo komunikuje v Internetu přes reverzní proxy server, například služby IIS, Nginx nebo Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="b914b-122">Buď konfiguraci &mdash; s nebo bez reverzní proxy server &mdash; lze také použít, pokud Kestrel má přístup pouze k interní síti.</span><span class="sxs-lookup"><span data-stu-id="b914b-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b914b-123">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="b914b-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b914b-124">Pokud vaše aplikace přijímá požadavky jenom z interní sítě, můžete použít Kestrel samostatně.</span><span class="sxs-lookup"><span data-stu-id="b914b-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel komunikuje přímo s interní sítě](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="b914b-126">Pokud jste vystavit aplikace k Internetu, musí používat službu IIS, Nginx nebo Apache jako *reverzní proxy server*.</span><span class="sxs-lookup"><span data-stu-id="b914b-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="b914b-127">Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování.</span><span class="sxs-lookup"><span data-stu-id="b914b-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel nepřímo komunikuje v Internetu přes reverzní proxy server, například služby IIS, Nginx nebo Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="b914b-129">Reverzní proxy server je vyžadována pro nasazení okraj (vystavený pro přenosy z Internetu) z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="b914b-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="b914b-130">Verze 1.x Kestrel nemají úplný doplněk obranu proti útokům.</span><span class="sxs-lookup"><span data-stu-id="b914b-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="b914b-131">To zahrnuje, avšak není omezeno na příslušné vypršení časových limitů, omezení velikosti a omezení počtu souběžných připojení.</span><span class="sxs-lookup"><span data-stu-id="b914b-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="b914b-132">Scénáře, který vyžaduje reverzní proxy server je, když máte více aplikací, které sdílejí stejnou adresu IP a portu spouští na jednom serveru.</span><span class="sxs-lookup"><span data-stu-id="b914b-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="b914b-133">To není možné s Kestrel přímo Kestrel, který nepodporuje sdílení stejné IP adresy a portu mezi více procesy.</span><span class="sxs-lookup"><span data-stu-id="b914b-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="b914b-134">Když konfigurujete Kestrel naslouchat na portu, zpracovává všechny přenosy pro tento port bez ohledu na hlavičku hostitele.</span><span class="sxs-lookup"><span data-stu-id="b914b-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="b914b-135">Reverzní proxy server, můžete sdílet porty musí předat pak Kestrel na jedinečné IP adresy a portu.</span><span class="sxs-lookup"><span data-stu-id="b914b-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="b914b-136">I když není požadovaných reverzní proxy server, pomocí jednoho může být vhodný z jiných důvodů:</span><span class="sxs-lookup"><span data-stu-id="b914b-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="b914b-137">Ho můžete omezit vystavených útoku.</span><span class="sxs-lookup"><span data-stu-id="b914b-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="b914b-138">Poskytuje další úroveň volitelné konfigurace a obrany.</span><span class="sxs-lookup"><span data-stu-id="b914b-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="b914b-139">Může integrovat lépe stávající infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="b914b-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="b914b-140">Usnadňují Vyrovnávání zatížení a nastavení protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="b914b-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="b914b-141">Pouze reverzní proxy server vyžaduje certifikát SSL a tento server může komunikovat s vašimi aplikací servery v interní síti pomocí prostý protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="b914b-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="b914b-142">Pokud nepoužíváte reverzní proxy server s hostitelem filtrování povoleno, je nutné povolit [hostitele filtrování](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="b914b-142">If you're not using a reverse proxy with host filtering enabled, you must enable [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="b914b-143">Jak používat Kestrel v aplikacích ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b914b-143">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b914b-144">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="b914b-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b914b-145">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) je součástí balíčku [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="b914b-145">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="b914b-146">Šablony projektů ASP.NET Core pomocí Kestrel ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="b914b-146">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="b914b-147">V *Program.cs*, kód zavolá metodu šablony `CreateDefaultBuilder`, který volá [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) na pozadí.</span><span class="sxs-lookup"><span data-stu-id="b914b-147">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="b914b-148">Pokud potřebujete nakonfigurovat možnosti Kestrel, zavolejte `UseKestrel` v *Program.cs* jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="b914b-148">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b914b-149">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="b914b-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b914b-150">Nainstalujte [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="b914b-150">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="b914b-151">Volání [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) rozšiřující metody na `WebHostBuilder` v vaše `Main` metoda, zadání žádné [Kestrel možnosti](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) , které potřebujete, jak je znázorněno v následujícím oddílu.</span><span class="sxs-lookup"><span data-stu-id="b914b-151">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="b914b-152">Možnosti kestrel</span><span class="sxs-lookup"><span data-stu-id="b914b-152">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b914b-153">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="b914b-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b914b-154">Kestrel webového serveru obsahuje možnosti konfigurace omezení, které jsou užitečné zejména v internetových nasazení.</span><span class="sxs-lookup"><span data-stu-id="b914b-154">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="b914b-155">Zde naleznete některá omezení, které můžete zadat:</span><span class="sxs-lookup"><span data-stu-id="b914b-155">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="b914b-156">Maximální počet klientských připojení</span><span class="sxs-lookup"><span data-stu-id="b914b-156">Maximum client connections</span></span>
- <span data-ttu-id="b914b-157">Velikost textu maximální požadavku</span><span class="sxs-lookup"><span data-stu-id="b914b-157">Maximum request body size</span></span>
- <span data-ttu-id="b914b-158">Minimální požadavek textu přenosová rychlost</span><span class="sxs-lookup"><span data-stu-id="b914b-158">Minimum request body data rate</span></span>

<span data-ttu-id="b914b-159">Nastavení těchto omezení a ostatní uživatele v `Limits` vlastnost [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) třídy.</span><span class="sxs-lookup"><span data-stu-id="b914b-159">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="b914b-160">`Limits` Vlastnost obsahuje instanci [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) třídy.</span><span class="sxs-lookup"><span data-stu-id="b914b-160">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="b914b-161">**Maximální počet klientských připojení**</span><span class="sxs-lookup"><span data-stu-id="b914b-161">**Maximum client connections**</span></span>

<span data-ttu-id="b914b-162">Maximální počet souběžných otevřete připojení TCP lze nastavit pro celou aplikaci s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="b914b-162">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="b914b-163">Je samostatný limit pro připojení, které byly upgradované z protokolu HTTP nebo HTTPS na jiný protokol (například pro objekty WebSockets požadavek).</span><span class="sxs-lookup"><span data-stu-id="b914b-163">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="b914b-164">Po upgradu připojení není započítané `MaxConcurrentConnections` limit.</span><span class="sxs-lookup"><span data-stu-id="b914b-164">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="b914b-165">Maximální počet připojení je neomezená (null) ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="b914b-165">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="b914b-166">**Velikost textu maximální požadavku**</span><span class="sxs-lookup"><span data-stu-id="b914b-166">**Maximum request body size**</span></span>

<span data-ttu-id="b914b-167">Výchozí velikost těla maximální požadavku je 30,000,000 bajtů, což je přibližně 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="b914b-167">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="b914b-168">Doporučený způsob přepsat omezení v aplikaci ASP.NET MVC základní se má používat [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atribut na metodu akce:</span><span class="sxs-lookup"><span data-stu-id="b914b-168">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="b914b-169">Tady je příklad, který ukazuje, jak nakonfigurovat omezení pro celou aplikaci, každou žádost:</span><span class="sxs-lookup"><span data-stu-id="b914b-169">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="b914b-170">Můžete přepsat nastavení konkrétního požadavku v middlewaru.:</span><span class="sxs-lookup"><span data-stu-id="b914b-170">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="b914b-171">Pokud se pokusíte nakonfigurovat limit na požadavek po spuštění aplikace čtení požadavku, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="b914b-171">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="b914b-172">Je `IsReadOnly` vlastnost, která se dozvíte, pokud `MaxRequestBodySize` vlastnost je ve stavu jen pro čtení, tzn. je příliš pozdě Konfigurace limitu.</span><span class="sxs-lookup"><span data-stu-id="b914b-172">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="b914b-173">**Minimální požadavek textu přenosová rychlost**</span><span class="sxs-lookup"><span data-stu-id="b914b-173">**Minimum request body data rate**</span></span>

<span data-ttu-id="b914b-174">Kestrel kontroluje každou sekundu v případě pochází data na určenou míru v bajtech za sekundu.</span><span class="sxs-lookup"><span data-stu-id="b914b-174">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="b914b-175">Pokud rychlost klesne pod minimální, vypršení časového limitu připojení. Poskytnutá lhůta je množství času, aby Kestrel dává klienta ke zvýšení jeho rychlost odesílání až minimální; rychlost, jakou kontrolována během této doby.</span><span class="sxs-lookup"><span data-stu-id="b914b-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="b914b-176">Období odkladu pomáhá zabránit, vyřazení připojení, které jsou původně odesílání dat s nízkou rychlostí z důvodu zpomalit TCP-start.</span><span class="sxs-lookup"><span data-stu-id="b914b-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="b914b-177">Výchozí minimální rychlost je 240 bajtů za sekundu, období odkladu 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="b914b-177">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="b914b-178">Minimální rychlost platí také pro odpověď.</span><span class="sxs-lookup"><span data-stu-id="b914b-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="b914b-179">Kód pro nastavení limitu požadavků a omezení odpovědi je stejný kromě nutnosti `RequestBody` nebo `Response` v názvech vlastností a rozhraní.</span><span class="sxs-lookup"><span data-stu-id="b914b-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="b914b-180">Tady je příklad, který ukazuje, jak konfigurovat minimální datové sazby v *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b914b-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="b914b-181">Sazby za žádosti můžete nakonfigurovat v middlewaru:</span><span class="sxs-lookup"><span data-stu-id="b914b-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="b914b-182">Informace o dalších možnostech Kestrel najdete v tématu následující třídy:</span><span class="sxs-lookup"><span data-stu-id="b914b-182">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="b914b-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="b914b-183">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="b914b-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="b914b-184">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="b914b-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="b914b-185">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b914b-186">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="b914b-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b914b-187">Informace o možnostech Kestrel najdete v tématu [KestrelServerOptions třída](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="b914b-187">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="b914b-188">Konfigurace koncového bodu</span><span class="sxs-lookup"><span data-stu-id="b914b-188">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b914b-189">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="b914b-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b914b-190">Ve výchozím nastavení ASP.NET Core váže k `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b914b-190">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="b914b-191">Konfigurace předpony adres URL a portů pro Kestrel tak, aby naslouchala na voláním `Listen` nebo `ListenUnixSocket` metody na `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="b914b-191">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="b914b-192">(`UseUrls`, `urls` argument příkazového řádku a proměnnou prostředí ASPNETCORE_URLS také pracovní ale mají omezení uvedeno [dále v tomto článku](#useurls-limitations).)</span><span class="sxs-lookup"><span data-stu-id="b914b-192">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="b914b-193">**Vytvoření vazby na soket TCP**</span><span class="sxs-lookup"><span data-stu-id="b914b-193">**Bind to a TCP socket**</span></span>

<span data-ttu-id="b914b-194">`Listen` Metoda váže na soket TCP a lambda možnosti vám umožní nakonfigurovat certifikát SSL:</span><span class="sxs-lookup"><span data-stu-id="b914b-194">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="b914b-195">Všimněte si, jak tento příklad konfiguruje SSL pro koncový bod konkrétní pomocí [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="b914b-195">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="b914b-196">Konfigurovat další nastavení Kestrel pro konkrétní koncové body, můžete použít stejné rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b914b-196">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an X.509 cert](../../includes/make-x509-cert.md)]

<span data-ttu-id="b914b-197">**Vytvoření vazby na soket systému Unix**</span><span class="sxs-lookup"><span data-stu-id="b914b-197">**Bind to a Unix socket**</span></span>

<span data-ttu-id="b914b-198">Můžete naslouchání pro zlepšení výkonu s Nginx, soketu Unix, jak je uvedeno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="b914b-198">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="b914b-199">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="b914b-199">**Port 0**</span></span>

<span data-ttu-id="b914b-200">Pokud zadáte číslo portu 0, Kestrel dynamicky sváže dostupný port.</span><span class="sxs-lookup"><span data-stu-id="b914b-200">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="b914b-201">Následující příklad ukazuje, jak určit port, který Kestrel ve skutečnosti vázaný za běhu:</span><span class="sxs-lookup"><span data-stu-id="b914b-201">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="b914b-202">**Omezení UseUrls**</span><span class="sxs-lookup"><span data-stu-id="b914b-202">**UseUrls limitations**</span></span>

<span data-ttu-id="b914b-203">Koncové body můžete nakonfigurovat pomocí volání `UseUrls` metoda nebo pomocí `urls` argument příkazového řádku nebo proměnná prostředí ASPNETCORE_URLS.</span><span class="sxs-lookup"><span data-stu-id="b914b-203">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="b914b-204">Tyto metody jsou užitečné, pokud chcete, aby váš kód pro práci se servery než Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b914b-204">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="b914b-205">Ale mějte na paměti tato omezení:</span><span class="sxs-lookup"><span data-stu-id="b914b-205">However, be aware of these limitations:</span></span>

* <span data-ttu-id="b914b-206">Pomocí těchto metod, nelze používat protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="b914b-206">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="b914b-207">Pokud používáte obě `Listen` metoda a `UseUrls`, `Listen` přepsat koncové body `UseUrls` koncové body.</span><span class="sxs-lookup"><span data-stu-id="b914b-207">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="b914b-208">**Konfigurace koncového bodu pro službu IIS**</span><span class="sxs-lookup"><span data-stu-id="b914b-208">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="b914b-209">Pokud používáte službu IIS, vazby adresu URL pro službu IIS přepsat žádné vazby, která jste nastavili voláním buď `Listen` nebo `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="b914b-209">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="b914b-210">Další informace najdete v tématu [Úvod k modulu jádra ASP.NET](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="b914b-210">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b914b-211">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="b914b-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b914b-212">Ve výchozím nastavení ASP.NET Core váže k `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b914b-212">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="b914b-213">Můžete nakonfigurovat předpony adres URL a portů pro Kestrel tak, aby naslouchala na pomocí `UseUrls` metoda rozšíření `urls` argument příkazového řádku nebo konfigurační systém ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b914b-213">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="b914b-214">Další informace o těchto metodách v tématu [hostitelský](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="b914b-214">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="b914b-215">Informace o fungování vazby adresy URL při použití IIS jako reverzní proxy server, najdete v článku [ASP.NET Core modulu](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="b914b-215">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="b914b-216">Předpony adres URL</span><span class="sxs-lookup"><span data-stu-id="b914b-216">URL prefixes</span></span>

<span data-ttu-id="b914b-217">Když zavoláte `UseUrls` nebo použít `urls` argument příkazového řádku nebo proměnná prostředí ASPNETCORE_URLS předpony adres URL může být v některém z následujících formátů.</span><span class="sxs-lookup"><span data-stu-id="b914b-217">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b914b-218">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="b914b-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b914b-219">Pouze předpony adres URL protokolu HTTP jsou platné; Kestrel nepodporuje SSL při konfiguraci adresy URL vazby pomocí `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="b914b-219">Only HTTP URL prefixes are valid; Kestrel doesn't support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="b914b-220">Adresu IPv4 s číslo portu</span><span class="sxs-lookup"><span data-stu-id="b914b-220">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="b914b-221">0.0.0.0 je zvláštní případ, která se sváže s všechny adresy IPv4.</span><span class="sxs-lookup"><span data-stu-id="b914b-221">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="b914b-222">Adresa IPv6 se číslo portu</span><span class="sxs-lookup"><span data-stu-id="b914b-222">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="b914b-223">[:] je ekvivalentem IPv6 IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="b914b-223">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="b914b-224">Název hostitele s číslem portu</span><span class="sxs-lookup"><span data-stu-id="b914b-224">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="b914b-225">Názvy hostitelů \*, a + nejsou speciální.</span><span class="sxs-lookup"><span data-stu-id="b914b-225">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="b914b-226">Všechno, co není rozpoznaný IP adresu nebo "localhost" vytvoří vazbu pro všechny IP adresy IPv6 a IPv4.</span><span class="sxs-lookup"><span data-stu-id="b914b-226">Anything that's not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="b914b-227">Pokud potřebujete vytvořit vazbu různé názvy hostitelů na různé aplikace ASP.NET Core na stejném portu, použijte [HTTP.sys](httpsys.md) nebo reverzní proxy server, jako jsou služby IIS, Nginx nebo Apache.</span><span class="sxs-lookup"><span data-stu-id="b914b-227">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="b914b-228">Pokud nepoužíváte reverzní proxy server s hostitelem filtrování povoleno, je nutné povolit [hostitele filtrování](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="b914b-228">If you're not using a reverse proxy with host filtering enabled, you must enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="b914b-229">Název "Localhost" s IP číslo nebo zpětné smyčky port s číslem portu</span><span class="sxs-lookup"><span data-stu-id="b914b-229">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="b914b-230">Když `localhost` není zadaný, Kestrel se pokusí vytvořit vazbu na rozhraní zpětné smyčky protokolu IPv4 a IPv6.</span><span class="sxs-lookup"><span data-stu-id="b914b-230">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="b914b-231">Pokud požadovaný port je používán jinou službou buď rozhraní zpětné smyčky, Kestrel se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="b914b-231">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="b914b-232">Pokud buď rozhraní zpětné smyčky je k dispozici z jiného důvodu (Většina běžně, protože protokol IPv6 není podporován), Kestrel protokoly upozornění.</span><span class="sxs-lookup"><span data-stu-id="b914b-232">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b914b-233">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="b914b-233">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="b914b-234">Adresu IPv4 s číslo portu</span><span class="sxs-lookup"><span data-stu-id="b914b-234">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="b914b-235">0.0.0.0 je zvláštní případ, která se sváže s všechny adresy IPv4.</span><span class="sxs-lookup"><span data-stu-id="b914b-235">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="b914b-236">Adresa IPv6 se číslo portu</span><span class="sxs-lookup"><span data-stu-id="b914b-236">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="b914b-237">[:] je ekvivalentem IPv6 IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="b914b-237">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="b914b-238">Název hostitele s číslem portu</span><span class="sxs-lookup"><span data-stu-id="b914b-238">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="b914b-239">Názvy hostitelů \*, a + nejsou speciální.</span><span class="sxs-lookup"><span data-stu-id="b914b-239">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="b914b-240">Váže všechno, co není rozpoznaný IP adresu nebo "localhost" pro všechny IP adresy IPv6 a IPv4.</span><span class="sxs-lookup"><span data-stu-id="b914b-240">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="b914b-241">Pokud potřebujete vytvořit vazbu různé názvy hostitelů na různé aplikace ASP.NET Core na stejném portu, použijte [WebListener](weblistener.md) nebo reverzní proxy server, jako jsou služby IIS, Nginx nebo Apache.</span><span class="sxs-lookup"><span data-stu-id="b914b-241">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="b914b-242">Název "Localhost" s IP číslo nebo zpětné smyčky port s číslem portu</span><span class="sxs-lookup"><span data-stu-id="b914b-242">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="b914b-243">Když `localhost` není zadaný, Kestrel se pokusí vytvořit vazbu na rozhraní zpětné smyčky protokolu IPv4 a IPv6.</span><span class="sxs-lookup"><span data-stu-id="b914b-243">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="b914b-244">Pokud požadovaný port je používán jinou službou buď rozhraní zpětné smyčky, Kestrel se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="b914b-244">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="b914b-245">Pokud buď rozhraní zpětné smyčky je k dispozici z jiného důvodu (Většina běžně, protože protokol IPv6 není podporován), Kestrel protokoly upozornění.</span><span class="sxs-lookup"><span data-stu-id="b914b-245">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="b914b-246">UNIX soketů</span><span class="sxs-lookup"><span data-stu-id="b914b-246">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="b914b-247">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="b914b-247">**Port 0**</span></span>

<span data-ttu-id="b914b-248">Pokud zadáte číslo portu 0, Kestrel dynamicky sváže dostupný port.</span><span class="sxs-lookup"><span data-stu-id="b914b-248">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="b914b-249">Vytvoření vazby na portu 0 je povoleno pro všechny název hostitele nebo IP adresu s výjimkou `localhost` název.</span><span class="sxs-lookup"><span data-stu-id="b914b-249">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="b914b-250">Následující příklad ukazuje, jak určit port, který Kestrel ve skutečnosti vázaný za běhu:</span><span class="sxs-lookup"><span data-stu-id="b914b-250">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="b914b-251">**Předpony adres URL pro protokol SSL**</span><span class="sxs-lookup"><span data-stu-id="b914b-251">**URL prefixes for SSL**</span></span>

<span data-ttu-id="b914b-252">Nezapomeňte zahrnout předpony adres URL s `https:` při volání `UseHttps` rozšíření metoda, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="b914b-252">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

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
> <span data-ttu-id="b914b-253">Protokol HTTPS a HTTP nemůže být hostovaná na stejném portu.</span><span class="sxs-lookup"><span data-stu-id="b914b-253">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an X.509 cert](../../includes/make-x509-cert.md)]

---

## <a name="host-filtering"></a><span data-ttu-id="b914b-254">Filtrování hostitele</span><span class="sxs-lookup"><span data-stu-id="b914b-254">Host filtering</span></span>

<span data-ttu-id="b914b-255">I když Kestrel podporuje konfigurace, například podle předpony `http://example.com:5000`, z velké části ignoruje název hostitele.</span><span class="sxs-lookup"><span data-stu-id="b914b-255">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, it largely ignores the host name.</span></span> <span data-ttu-id="b914b-256">Localhost je zvláštní případ použité pro vazbu na adresy zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="b914b-256">Localhost is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="b914b-257">Žádný hostitel, jiné než explicitní IP adresu se váže k všechny veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="b914b-257">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="b914b-258">Žádná z těchto informací se používá k ověření hlavičky hostitele požadavku.</span><span class="sxs-lookup"><span data-stu-id="b914b-258">None of this information is used to validate request Host headers.</span></span>

<span data-ttu-id="b914b-259">Existují dvě možná řešení:</span><span class="sxs-lookup"><span data-stu-id="b914b-259">There are two possible workarounds:</span></span>

* <span data-ttu-id="b914b-260">Hostitel za reverzní proxy server s filtrování hlavičky hostitele.</span><span class="sxs-lookup"><span data-stu-id="b914b-260">Host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="b914b-261">To byl jediný podporovaný scénář pro Kestrel v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="b914b-261">This was the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>
* <span data-ttu-id="b914b-262">Pomocí middleware filtrovat požadavky v hlavičce hostitele.</span><span class="sxs-lookup"><span data-stu-id="b914b-262">Use a middleware to filter requests by the host header.</span></span> <span data-ttu-id="b914b-263">Následuje ukázka middleware:</span><span class="sxs-lookup"><span data-stu-id="b914b-263">A sample middleware follows:</span></span>

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
            _logger.LogDebug("Request rejected due to incorrect host header.");
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
            // Http/1.0 does not require the host header.
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

<span data-ttu-id="b914b-264">Zaregistrovat předchozím `HostFilteringMiddleware` v `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="b914b-264">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="b914b-265">Všimněte si, že [řazení middleware registrace](xref:fundamentals/middleware/index#ordering) je důležité.</span><span class="sxs-lookup"><span data-stu-id="b914b-265">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#ordering) is important.</span></span> <span data-ttu-id="b914b-266">K registraci by mělo dojít ihned po diagnostiky middleware (například `app.UseExceptionHandler`).</span><span class="sxs-lookup"><span data-stu-id="b914b-266">Registration should occur immediately after the diagnostic middleware (for example, `app.UseExceptionHandler`).</span></span>

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

<span data-ttu-id="b914b-267">Předchozí middleware očekává `AllowedHosts` klíče v *appsettings.\< EnvironmentName > .json*.</span><span class="sxs-lookup"><span data-stu-id="b914b-267">The preceding middleware expects an `AllowedHosts` key in *appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="b914b-268">Hodnota tohoto klíče je seznam názvů hostitele bez čísla portů oddělených středníky.</span><span class="sxs-lookup"><span data-stu-id="b914b-268">This key's value is a semicolon-delimited list of host names without the port numbers.</span></span> <span data-ttu-id="b914b-269">Zahrnout `AllowedHosts` dvojice klíč hodnota v *appsettings. Production.JSON*:</span><span class="sxs-lookup"><span data-stu-id="b914b-269">Include the `AllowedHosts` key-value pair in *appsettings.Production.json*:</span></span>

```json
{
  "AllowedHosts": "example.com"
}
```

<span data-ttu-id="b914b-270">Konfigurační soubor localhost *appsettings. Development.JSON*, vypadá podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="b914b-270">The localhost configuration file, *appsettings.Development.json*, looks like this:</span></span>

```json
{
  "AllowedHosts": "localhost"
}
```

## <a name="next-steps"></a><span data-ttu-id="b914b-271">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b914b-271">Next steps</span></span>

<span data-ttu-id="b914b-272">Další informace naleznete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="b914b-272">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b914b-273">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="b914b-273">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="b914b-274">Ukázková aplikace pro 2.x</span><span class="sxs-lookup"><span data-stu-id="b914b-274">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="b914b-275">Kestrel zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="b914b-275">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b914b-276">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="b914b-276">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="b914b-277">Ukázková aplikace pro 1.x</span><span class="sxs-lookup"><span data-stu-id="b914b-277">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="b914b-278">Kestrel zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="b914b-278">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
