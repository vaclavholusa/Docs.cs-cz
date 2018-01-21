---
title: "Implementace webového serveru v ASP.NET Core"
author: tdykstra
description: "Zavádí webové servery Kestrel a WebListener pro ASP.NET Core. Poskytuje pokyny o tom, jak vyberte jeden a jeho použití s reverzní proxy server."
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/index
ms.openlocfilehash: 807e60e61d4ce4d5755987cffe65d130c9bbbd42
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="45be2-104">Implementace webového serveru v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="45be2-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="45be2-105">Podle [tní Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), a [Ross Jan](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="45be2-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="45be2-106">Aplikace ASP.NET Core pracuje s implementaci serveru HTTP v procesu.</span><span class="sxs-lookup"><span data-stu-id="45be2-106">An ASP.NET Core application runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="45be2-107">Implementace server naslouchá pro požadavky protokolu HTTP a zobrazí je do aplikace jako sady [žádosti o funkce](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) složený do `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="45be2-107">The server implementation listens for HTTP requests and surfaces them to the application as sets of [request features](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) composed into an `HttpContext`.</span></span>

<span data-ttu-id="45be2-108">ASP.NET Core dodává dvě implementace serveru:</span><span class="sxs-lookup"><span data-stu-id="45be2-108">ASP.NET Core ships two server implementations:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="45be2-109">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="45be2-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="45be2-110">[Kestrel](kestrel.md) je na základě server HTTP napříč platformami [libuv](https://github.com/libuv/libuv), knihovny a platformy asynchronní vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="45be2-110">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="45be2-111">[Ovladač HTTP.sys](httpsys.md) je na základě protokolu HTTP pouze pro systém Windows server [ovladač Http.Sys jádra](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="45be2-111">[HTTP.sys](httpsys.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="45be2-112">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="45be2-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="45be2-113">[Kestrel](kestrel.md) je na základě server HTTP napříč platformami [libuv](https://github.com/libuv/libuv), knihovny a platformy asynchronní vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="45be2-113">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="45be2-114">[WebListener](weblistener.md) je na základě protokolu HTTP pouze pro systém Windows server [ovladač Http.Sys jádra](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="45be2-114">[WebListener](weblistener.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

---

## <a name="kestrel"></a><span data-ttu-id="45be2-115">Kestrel</span><span class="sxs-lookup"><span data-stu-id="45be2-115">Kestrel</span></span>

<span data-ttu-id="45be2-116">Kestrel je webový server, který je zahrnut ve výchozím nastavení v šablonách nový projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="45be2-116">Kestrel is the web server that is included by default in ASP.NET Core new-project templates.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="45be2-117">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="45be2-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="45be2-118">Kestrel můžete použít samostatně nebo se *reverzní proxy server*, jako jsou například služby IIS, Nginx nebo Apache.</span><span class="sxs-lookup"><span data-stu-id="45be2-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="45be2-119">Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování.</span><span class="sxs-lookup"><span data-stu-id="45be2-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel komunikuje přímo s Internetu bez reverzní proxy server](kestrel/_static/kestrel-to-internet2.png)

![Kestrel nepřímo komunikuje v Internetu přes reverzní proxy server, například služby IIS, Nginx nebo Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="45be2-122">Buď konfiguraci &mdash; s nebo bez reverzní proxy server &mdash; lze také použít, pokud Kestrel má přístup pouze k interní síti.</span><span class="sxs-lookup"><span data-stu-id="45be2-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

<span data-ttu-id="45be2-123">Informace o použití Kestrel s reverzní proxy server, najdete v článku [Úvod do Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="45be2-123">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="45be2-124">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="45be2-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="45be2-125">Pokud vaše aplikace přijímá požadavky jenom z interní sítě, můžete použít Kestrel samostatně.</span><span class="sxs-lookup"><span data-stu-id="45be2-125">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel komunikuje přímo s interní sítě](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="45be2-127">Pokud jste vystavit aplikace k Internetu, musí používat službu IIS, Nginx nebo Apache jako *reverzní proxy server*.</span><span class="sxs-lookup"><span data-stu-id="45be2-127">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="45be2-128">Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování, jak je znázorněno v následujícím diagramu.</span><span class="sxs-lookup"><span data-stu-id="45be2-128">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram.</span></span>

![Kestrel nepřímo komunikuje v Internetu přes reverzní proxy server, například služby IIS, Nginx nebo Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="45be2-130">Nejdůležitější důvod pro používání reverzní proxy server pro nasazení okraj (vystavený pro přenosy z Internetu) je zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="45be2-130">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="45be2-131">Verze 1.x Kestrel nemají úplný doplněk obranu proti útokům.</span><span class="sxs-lookup"><span data-stu-id="45be2-131">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="45be2-132">To zahrnuje, ale není omezen na, příslušné vypršení časových limitů, omezení velikosti a omezení počtu souběžných připojení.</span><span class="sxs-lookup"><span data-stu-id="45be2-132">This includes, but isn't limited to, appropriate timeouts, size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="45be2-133">Informace o použití Kestrel s reverzní proxy server, najdete v článku [Úvod do Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="45be2-133">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

---

<span data-ttu-id="45be2-134">Nelze použít IIS, Nginx nebo Apache bez Kestrel nebo [implementace vlastního serveru](#custom-servers).</span><span class="sxs-lookup"><span data-stu-id="45be2-134">You can't use IIS, Nginx, or Apache without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="45be2-135">ASP.NET Core byl navržen ke spuštění ve svém vlastním procesu, takže může chovat konzistentně napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="45be2-135">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="45be2-136">Služba IIS, Nginx a Apache stanovují vlastní procesu spouštění a prostředí; používat přímo, ASP.NET Core muset přizpůsobit potřebám každé z nich.</span><span class="sxs-lookup"><span data-stu-id="45be2-136">IIS, Nginx, and Apache dictate their own startup process and environment; to use them directly, ASP.NET Core would have to adapt to the needs of each one.</span></span> <span data-ttu-id="45be2-137">Pomocí na webový server implementace například Kestrel nabízí ASP.NET Core kontrolu nad spuštění procesu a prostředí.</span><span class="sxs-lookup"><span data-stu-id="45be2-137">Using a web server implementation such as Kestrel gives ASP.NET Core control over the startup process and environment.</span></span> <span data-ttu-id="45be2-138">Proto namísto pokusu přizpůsobit ASP.NET Core služby IIS, Nginx nebo Apache, je právě nastavit tyto webové servery proxy požadavky na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="45be2-138">So rather than trying to adapt ASP.NET Core to IIS, Nginx, or Apache, you just set up those web servers to proxy requests to Kestrel.</span></span> <span data-ttu-id="45be2-139">Toto uspořádání umožňuje vaší `Program.Main` a `Startup` třídy být v podstatě stejný bez ohledu na to, kde můžete nasadit.</span><span class="sxs-lookup"><span data-stu-id="45be2-139">This arrangement allows your `Program.Main` and `Startup` classes to be essentially the same no matter where you deploy.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="45be2-140">IIS s Kestrel</span><span class="sxs-lookup"><span data-stu-id="45be2-140">IIS with Kestrel</span></span>

<span data-ttu-id="45be2-141">Při použití služby IIS nebo IIS Express jako reverzní proxy server pro ASP.NET Core aplikace ASP.NET Core spouští samostatný proces z pracovní proces služby IIS.</span><span class="sxs-lookup"><span data-stu-id="45be2-141">When you use IIS or IIS Express as a reverse proxy for ASP.NET Core, the ASP.NET Core application runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="45be2-142">V procesu služby IIS spouští speciální modul IIS ke koordinaci relace reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="45be2-142">In the IIS process, a special IIS module runs to coordinate the reverse proxy relationship.</span></span>  <span data-ttu-id="45be2-143">Toto je *ASP.NET Core modulu*.</span><span class="sxs-lookup"><span data-stu-id="45be2-143">This is the *ASP.NET Core Module*.</span></span> <span data-ttu-id="45be2-144">Primární funkce modulu jádra ASP.NET se spustit aplikaci ASP.NET Core, ho restartovat, když ho dojde k chybě a přenosu dat protokolu HTTP k němu.</span><span class="sxs-lookup"><span data-stu-id="45be2-144">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core application, restart it when it crashes, and forward HTTP traffic to it.</span></span> <span data-ttu-id="45be2-145">Další informace najdete v tématu [ASP.NET Core modulu](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="45be2-145">For more information, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

### <a name="nginx-with-kestrel"></a><span data-ttu-id="45be2-146">Nginx s Kestrel</span><span class="sxs-lookup"><span data-stu-id="45be2-146">Nginx with Kestrel</span></span>

<span data-ttu-id="45be2-147">Informace o tom, jak používat Nginx v systému Linux jako reverzní proxy server pro Kestrel najdete v tématu [hostitele v systému Linux s Nginx](xref:host-and-deploy/linux-nginx).</span><span class="sxs-lookup"><span data-stu-id="45be2-147">For information about how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="45be2-148">Apache s Kestrel</span><span class="sxs-lookup"><span data-stu-id="45be2-148">Apache with Kestrel</span></span>

<span data-ttu-id="45be2-149">Informace o tom, jak používat Apache v systému Linux jako reverzní proxy server pro Kestrel najdete v tématu [hostitele v systému Linux s Apache](xref:host-and-deploy/linux-apache).</span><span class="sxs-lookup"><span data-stu-id="45be2-149">For information about how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="45be2-150">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="45be2-150">HTTP.sys</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="45be2-151">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="45be2-151">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="45be2-152">Pokud vaše aplikace ASP.NET Core spustíte v systému Windows, ovladač HTTP.sys je alternativa k Kestrel.</span><span class="sxs-lookup"><span data-stu-id="45be2-152">If you run your ASP.NET Core app on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="45be2-153">Ovladač HTTP.sys můžete použít pro scénáře, kde vystavit aplikace k Internetu a budete potřebovat HTTP.sys funkce, které Kestrel nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="45be2-153">You can use HTTP.sys for scenarios where you expose your app to the Internet and you need HTTP.sys features that Kestrel doesn't support.</span></span> 

![Ovladač HTTP.sys komunikuje přímo přes Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="45be2-155">Ovladač HTTP.sys mohou sloužit také pro aplikace, které jsou viditelné pouze na interní síti.</span><span class="sxs-lookup"><span data-stu-id="45be2-155">HTTP.sys can also be used for applications that are exposed only to an internal network.</span></span> 

![Ovladač HTTP.sys komunikuje přímo s interní sítě](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="45be2-157">Pro scénáře interní síti Kestrel obecně doporučujeme pro nejlepší výkon; ale v některých případech můžete chtít použít funkci, která nabízí jenom ovladače HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="45be2-157">For internal network scenarios, Kestrel is generally recommended for best performance; but in some scenarios, you might want to use a feature that only HTTP.sys offers.</span></span> <span data-ttu-id="45be2-158">Informace o funkcích, ovladač HTTP.sys najdete v tématu [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="45be2-158">For information about HTTP.sys features, see [HTTP.sys](httpsys.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="45be2-159">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="45be2-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="45be2-160">Ovladač HTTP.sys jmenuje WebListener v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="45be2-160">HTTP.sys is named WebListener in ASP.NET Core 1.x.</span></span> <span data-ttu-id="45be2-161">Pokud spustíte vaše základní technologie ASP.NET je aplikace v systému Windows, WebListener alternativu, která můžete použít pro scénáře, kde chcete vystavit v aplikaci Internet, ale nelze použít IIS.</span><span class="sxs-lookup"><span data-stu-id="45be2-161">If you run your ASP.NET Core app on Windows, WebListener is an alternative that you can use for scenarios where you want to expose your app to the Internet but you can't use IIS.</span></span>

![Weblistener komunikuje přímo přes Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="45be2-163">WebListener také jde použít místo Kestrel pro aplikace, které jsou viditelné pouze na interní síti, pokud potřebujete WebListener funkce, které Kestrel nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="45be2-163">WebListener can also be used in place of Kestrel for applications that are exposed only to an internal network, if you need WebListener features that Kestrel doesn't support.</span></span> 

![Weblistener komunikuje přímo s interní sítě](weblistener/_static/weblistener-to-internal.png)

<span data-ttu-id="45be2-165">Pro scénáře interní síti Kestrel obecně doporučujeme pro nejlepší výkon, ale v některých případech můžete chtít použít funkci, která nabízí pouze WebListener.</span><span class="sxs-lookup"><span data-stu-id="45be2-165">For internal network scenarios, Kestrel is generally recommended for best performance, but in some scenarios you might want to use a feature that only WebListener offers.</span></span> <span data-ttu-id="45be2-166">Informace o funkcích WebListener najdete v tématu [WebListener](weblistener.md).</span><span class="sxs-lookup"><span data-stu-id="45be2-166">For information about WebListener features, see [WebListener](weblistener.md).</span></span>

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a><span data-ttu-id="45be2-167">Poznámky k nástroji ASP.NET Core serveru infrastruktury</span><span class="sxs-lookup"><span data-stu-id="45be2-167">Notes about ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="45be2-168">[ `IApplicationBuilder` ](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) k dispozici v `Startup` třída `Configure` metoda zpřístupňuje `ServerFeatures` vlastnost typu [ `IFeatureCollection` ](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="45be2-168">The [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup` class `Configure` method exposes the `ServerFeatures` property of type [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="45be2-169">Kestrel a WebListener vystavují jenom jednu funkci [ `IServerAddressesFeature` ](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ale implementace jiný server může vystavit další funkce.</span><span class="sxs-lookup"><span data-stu-id="45be2-169">Kestrel and WebListener both expose only a single feature, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="45be2-170">`IServerAddressesFeature`umožňuje zjistit, který port implementaci serveru je vázána na za běhu.</span><span class="sxs-lookup"><span data-stu-id="45be2-170">`IServerAddressesFeature` can be used to find out which port the server implementation has bound to at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="45be2-171">Vlastní servery</span><span class="sxs-lookup"><span data-stu-id="45be2-171">Custom servers</span></span>

<span data-ttu-id="45be2-172">Pokud integrované servery nevyhovují vašim potřebám, můžete vytvořit vlastní server implementace.</span><span class="sxs-lookup"><span data-stu-id="45be2-172">If the built-in servers don't meet your needs, you can create a custom server implementation.</span></span> <span data-ttu-id="45be2-173">[Open Web Interface pro .NET (OWIN) průvodce](../owin.md) ukazuje, jak napsat [Nowin](https://github.com/Bobris/Nowin)– na základě [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementace.</span><span class="sxs-lookup"><span data-stu-id="45be2-173">The [Open Web Interface for .NET (OWIN) guide](../owin.md) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="45be2-174">Jste volné implementovat právě funkce rozhraní aplikace potřebuje, když minimálně musí podporovat [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) a [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span><span class="sxs-lookup"><span data-stu-id="45be2-174">You're free to implement just the feature interfaces your application needs, though at a minimum you must support [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span></span>

## <a name="next-steps"></a><span data-ttu-id="45be2-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="45be2-175">Next steps</span></span>

<span data-ttu-id="45be2-176">Další informace naleznete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="45be2-176">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="45be2-177">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="45be2-177">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

- [<span data-ttu-id="45be2-178">Kestrel</span><span class="sxs-lookup"><span data-stu-id="45be2-178">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="45be2-179">Kestrel službou IIS</span><span class="sxs-lookup"><span data-stu-id="45be2-179">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="45be2-180">Hostování v Linuxu na serveru Nginx</span><span class="sxs-lookup"><span data-stu-id="45be2-180">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="45be2-181">Hostování v Linuxu na serveru Apache</span><span class="sxs-lookup"><span data-stu-id="45be2-181">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="45be2-182">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="45be2-182">HTTP.sys</span></span>](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="45be2-183">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="45be2-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

- [<span data-ttu-id="45be2-184">Kestrel</span><span class="sxs-lookup"><span data-stu-id="45be2-184">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="45be2-185">Kestrel službou IIS</span><span class="sxs-lookup"><span data-stu-id="45be2-185">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="45be2-186">Hostování v Linuxu na serveru Nginx</span><span class="sxs-lookup"><span data-stu-id="45be2-186">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="45be2-187">Hostování v Linuxu na serveru Apache</span><span class="sxs-lookup"><span data-stu-id="45be2-187">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="45be2-188">WebListener</span><span class="sxs-lookup"><span data-stu-id="45be2-188">WebListener</span></span>](weblistener.md)

---
