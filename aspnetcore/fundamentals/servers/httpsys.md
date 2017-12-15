---
title: "Ovladač HTTP.sys webového serveru implementace v ASP.NET Core"
author: rick-anderson
description: "Představuje HTTP.sys, webový server pro ASP.NET Core v systému Windows. Postavená na ovladač režimu jádra Http.Sys, ovladač HTTP.sys je alternativa k Kestrel, který lze použít pro přímé připojení k Internetu bez služby IIS."
keywords: "ASP.NET Core,HttpSys,HTTP.sys,HttpListener,url předpony, protokol SSL"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 8d46862af44379d8592efdf214a80214dce2d69d
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="56445-105">Ovladač HTTP.sys webového serveru implementace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56445-105">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="56445-106">Podle [tní Dykstra](https://github.com/tdykstra) a [Ross Jan](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="56445-106">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="56445-107">Toto téma se vztahuje pouze na technologii ASP.NET 2.0 jádra a novější.</span><span class="sxs-lookup"><span data-stu-id="56445-107">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="56445-108">V dřívějších verzích ASP.NET Core, ovladač HTTP.sys jmenuje [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="56445-108">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="56445-109">Je ovladač HTTP.sys [webového serveru pro ASP.NET Core](index.md) používající pouze v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="56445-109">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="56445-110">Je založen na [ovladač režimu jádra Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="56445-110">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="56445-111">Ovladač HTTP.sys je alternativa k [Kestrel](kestrel.md) , nabízí některé funkce, které není Kestel.</span><span class="sxs-lookup"><span data-stu-id="56445-111">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="56445-112">**Ovladač HTTP.sys nelze použít s služby IIS nebo IIS Express, jako je nekompatibilní s [ASP.NET Core modulu](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="56445-112">**HTTP.sys can't be used with IIS or IIS Express, as it's incompatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="56445-113">Ovladač HTTP.sys podporuje následující funkce:</span><span class="sxs-lookup"><span data-stu-id="56445-113">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="56445-114">Ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="56445-114">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="56445-115">Sdílení portů</span><span class="sxs-lookup"><span data-stu-id="56445-115">Port sharing</span></span>
- <span data-ttu-id="56445-116">HTTPS s SNI</span><span class="sxs-lookup"><span data-stu-id="56445-116">HTTPS with SNI</span></span>
- <span data-ttu-id="56445-117">HTTP/2 přes protokol TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="56445-117">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="56445-118">Přímé souboru přenosu</span><span class="sxs-lookup"><span data-stu-id="56445-118">Direct file transmission</span></span>
- <span data-ttu-id="56445-119">Ukládání odpovědí do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="56445-119">Response caching</span></span>
- <span data-ttu-id="56445-120">Technologie WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="56445-120">WebSockets (Windows 8)</span></span>

<span data-ttu-id="56445-121">Podporované verze systému Windows:</span><span class="sxs-lookup"><span data-stu-id="56445-121">Supported Windows versions:</span></span>

- <span data-ttu-id="56445-122">Windows 7 a Windows Server 2008 R2 a novější</span><span class="sxs-lookup"><span data-stu-id="56445-122">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="56445-123">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="56445-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="56445-124">Kdy použít ovladače HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="56445-124">When to use HTTP.sys</span></span>

<span data-ttu-id="56445-125">Ovladač HTTP.sys je užitečná pro nasazení, kde je nutné vystavit server přímo k Internetu bez použití služby IIS.</span><span class="sxs-lookup"><span data-stu-id="56445-125">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Ovladač HTTP.sys komunikuje přímo přes Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="56445-127">Protože je založen na Http.Sys, ovladač HTTP.sys nevyžaduje reverzní proxy server pro ochranu před útoky.</span><span class="sxs-lookup"><span data-stu-id="56445-127">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="56445-128">Ovladač Http.Sys je Vyspělá technologie, která chrání před různé druhy útoky a poskytuje odolnost, zabezpečení a škálovatelnost plné webového serveru.</span><span class="sxs-lookup"><span data-stu-id="56445-128">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="56445-129">Služba IIS spouští jako naslouchací proces protokolu HTTP na základě ovladače Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="56445-129">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="56445-130">Ovladače HTTP.sys je vhodná pro interní nasazení, když potřebujete funkce není k dispozici v Kestrel, jako je například ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="56445-130">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![Ovladač HTTP.sys komunikuje přímo s interní sítě](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="56445-132">Jak používat ovladač HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="56445-132">How to use HTTP.sys</span></span>

<span data-ttu-id="56445-133">Zde je uveden přehled úloh nastavení pro hostitelský operační systém a aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="56445-133">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="56445-134">Konfigurace Windows serveru</span><span class="sxs-lookup"><span data-stu-id="56445-134">Configure Windows Server</span></span>

* <span data-ttu-id="56445-135">Nainstalujte verzi rozhraní .NET, který vyžaduje vaše aplikace, jako například [.NET Core](https://www.microsoft.com/net/download/core) nebo [rozhraní .NET Framework](https://www.microsoft.com/net/download/framework).</span><span class="sxs-lookup"><span data-stu-id="56445-135">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="56445-136">Preregister předpony adres URL pro svázání ovladače HTTP.sys a nastavení certifikátů SSL</span><span class="sxs-lookup"><span data-stu-id="56445-136">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="56445-137">Pokud nemáte preregister předpony adres URL v systému Windows, budete muset aplikaci spustit s oprávněním správce.</span><span class="sxs-lookup"><span data-stu-id="56445-137">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="56445-138">Jedinou výjimkou je, pokud vytvoření vazby na localhost pomocí protokolu HTTP (nikoli HTTPS) se číslo portu větší než 1024; v takovém případě se vyžaduje oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="56445-138">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="56445-139">Podrobnosti najdete v tématu [preregister předpony a konfigurace protokolu SSL](#preregister-url-prefixes-and-configure-ssl) dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="56445-139">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="56445-140">Otevřete porty brány firewall umožňující přenos k dosažení ovladače HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="56445-140">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="56445-141">Můžete použít *netsh.exe* nebo [rutiny prostředí PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="56445-141">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="56445-142">Existují také [nastavení registru Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="56445-142">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="56445-143">Konfiguraci aplikace ASP.NET Core používat ovladač HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="56445-143">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="56445-144">Pokud používáte je nutná žádná instalace balíčku [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="56445-144">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="56445-145">[Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) balíček je součástí metapackage.</span><span class="sxs-lookup"><span data-stu-id="56445-145">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="56445-146">Volání `UseHttpSys` rozšiřující metody na `WebHostBuilder` v vaše `Main` metoda, zadání žádné [HTTP.sys možnosti](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) , které potřebujete, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="56445-146">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="56445-147">Konfigurace možností HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="56445-147">Configure HTTP.sys options</span></span>

<span data-ttu-id="56445-148">Zde jsou některé z ovladače HTTP.sys nastavení a omezení, která se dají konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="56445-148">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="56445-149">**Maximální počet klientských připojení**</span><span class="sxs-lookup"><span data-stu-id="56445-149">**Maximum client connections**</span></span>

<span data-ttu-id="56445-150">Maximální počet souběžných otevřete připojení TCP lze nastavit pro celou aplikaci s následujícím kódem v *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="56445-150">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="56445-151">Maximální počet připojení je neomezená (null) ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="56445-151">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="56445-152">**Velikost textu maximální požadavku**</span><span class="sxs-lookup"><span data-stu-id="56445-152">**Maximum request body size**</span></span>

<span data-ttu-id="56445-153">Výchozí velikost těla maximální požadavku je 30,000,000 bajtů, což je přibližně 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="56445-153">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="56445-154">Doporučený způsob přepsat omezení v aplikaci ASP.NET MVC základní se má používat [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atribut na metodu akce:</span><span class="sxs-lookup"><span data-stu-id="56445-154">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="56445-155">Tady je příklad, který ukazuje, jak nakonfigurovat omezení pro celou aplikaci, každou žádost:</span><span class="sxs-lookup"><span data-stu-id="56445-155">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="56445-156">Můžete přepsat nastavení konkrétního požadavku v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="56445-156">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="56445-157">Pokud se pokusíte nakonfigurovat limit na požadavek po spuštění aplikace čtení požadavku, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="56445-157">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="56445-158">Je `IsReadOnly` vlastnost, která se dozvíte, pokud `MaxRequestBodySize` vlastnost je ve stavu jen pro čtení, tzn. je příliš pozdě Konfigurace limitu.</span><span class="sxs-lookup"><span data-stu-id="56445-158">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="56445-159">Informace o dalších možnostech HTTP.sys najdete v tématu [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="56445-159">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="56445-160">Konfigurace adresy URL a portů pro naslouchání</span><span class="sxs-lookup"><span data-stu-id="56445-160">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="56445-161">Ve výchozím nastavení ASP.NET Core váže k `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="56445-161">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="56445-162">Konfigurace předpony adres URL a portů, můžete použít `UseUrls` metoda rozšíření `urls` argument příkazového řádku, proměnnou prostředí ASPNETCORE_URLS nebo `UrlPrefixes` vlastnost [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="56445-162">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="56445-163">Následující příklad kódu používá `UrlPrefixes`.</span><span class="sxs-lookup"><span data-stu-id="56445-163">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="56445-164">Výhodou `UrlPrefixes` je, že dostanete chybovou zprávu okamžitě pokud se pokusíte přidat předponu, která je špatně naformátovaný.</span><span class="sxs-lookup"><span data-stu-id="56445-164">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that is formatted wrong.</span></span> <span data-ttu-id="56445-165">Výhodou `UseUrls` (sdíleny s `urls` a ASPNETCORE_URLS) je, že můžete snadno přepínat mezi Kestrel a ovladače HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="56445-165">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="56445-166">Pokud používáte obě `UseUrls` (nebo `urls` nebo ASPNETCORE_URLS) a `UrlPrefixes`, nastavení v `UrlPrefixes` přepsat těm, které jsou v `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="56445-166">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="56445-167">Další informace najdete v tématu [hostitelský](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="56445-167">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="56445-168">Používá ovladače HTTP.sys [formáty řetězců UrlPrefix rozhraní API serveru HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="56445-168">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="56445-169">Ujistěte se, zda jste zadali stejné předpona řetězce v `UseUrls` nebo `UrlPrefixes` , preregister na serveru.</span><span class="sxs-lookup"><span data-stu-id="56445-169">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="56445-170">Nepoužívejte služby IIS</span><span class="sxs-lookup"><span data-stu-id="56445-170">Don't use IIS</span></span>

<span data-ttu-id="56445-171">Ujistěte se, že vaše aplikace není nakonfigurována pro spuštění služby IIS nebo IIS Express.</span><span class="sxs-lookup"><span data-stu-id="56445-171">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="56445-172">V sadě Visual Studio je výchozí profil spuštění pro službu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="56445-172">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="56445-173">Chcete-li spustit projekt jako konzolové aplikace, ručně změňte vybraný profil, jak je znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="56445-173">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![Vyberte profil aplikace konzoly](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="56445-175">Preregister předpony adres URL a konfigurace protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="56445-175">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="56445-176">Služba IIS a ovladač HTTP.sys závisí na základní ovladač režimu jádra Http.Sys naslouchat požadavkům a počáteční zpracování.</span><span class="sxs-lookup"><span data-stu-id="56445-176">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="56445-177">Ve službě IIS uživatelské rozhraní pro správu poskytuje relativně snadný způsob, jak nakonfigurovat vše, co.</span><span class="sxs-lookup"><span data-stu-id="56445-177">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="56445-178">Však musíte nakonfigurovat Http.Sys sami.</span><span class="sxs-lookup"><span data-stu-id="56445-178">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="56445-179">Integrované nástroje způsobem, který je *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="56445-179">The built-in tool for doing that is *netsh.exe*.</span></span> 

<span data-ttu-id="56445-180">S *netsh.exe* můžete vyhradit předpony adres URL a přiřadit certifikáty SSL.</span><span class="sxs-lookup"><span data-stu-id="56445-180">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="56445-181">Tento nástroj vyžaduje oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="56445-181">The tool requires administrative privileges.</span></span>

<span data-ttu-id="56445-182">Následující příklad ukazuje minimální počet potřebný pro rezervovat předpony adres URL pro porty 80 a 443:</span><span class="sxs-lookup"><span data-stu-id="56445-182">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="56445-183">Následující příklad ukazuje, jak přiřadit certifikát SSL:</span><span class="sxs-lookup"><span data-stu-id="56445-183">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="56445-184">Tady je referenční dokumentaci k nástroji pro *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="56445-184">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="56445-185">Příkazy Netsh pro Hypertext Transfer Protocol (HTTP)</span><span class="sxs-lookup"><span data-stu-id="56445-185">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="56445-186">UrlPrefix řetězce</span><span class="sxs-lookup"><span data-stu-id="56445-186">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="56445-187">Následující prostředky poskytují podrobné pokyny pro několik scénářů.</span><span class="sxs-lookup"><span data-stu-id="56445-187">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="56445-188">Články, které odkazují na HttpListener použít ovladače HTTP.sys, stejně jako obě jsou založené na ovladače Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="56445-188">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="56445-189">Postupy: Konfigurace portu s certifikátem SSL</span><span class="sxs-lookup"><span data-stu-id="56445-189">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="56445-190">[Hostitelský a certifikát klienta na základě komunikaci pomocí protokolu HTTPS - HttpListener](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) to je blog třetí strany a je docela v minulosti, ale má stále užitečné informace.</span><span class="sxs-lookup"><span data-stu-id="56445-190">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="56445-191">[Postupy: Použití HttpListener návod nebo Http Server nespravovaného kódu (C++) jako Server jednoduché SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) příliš jde starší blog s užitečné informace.</span><span class="sxs-lookup"><span data-stu-id="56445-191">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="56445-192">Tady jsou některé nástroje třetích stran, které mohou být jednodušší než *netsh.exe* příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="56445-192">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="56445-193">Tyto jsou poskytované nebo schválené společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="56445-193">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="56445-194">Nástroje spustit jako správce ve výchozím nastavení, protože *netsh.exe* samotné vyžaduje oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="56445-194">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="56445-195">[ovladač HTTP.sys Manager](http://httpsysmanager.codeplex.com/) poskytuje uživatelské rozhraní pro výpis a konfiguraci certifikátů SSL a možnosti, předpony rezervace a seznamy důvěryhodných certifikátů.</span><span class="sxs-lookup"><span data-stu-id="56445-195">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="56445-196">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) umožňuje seznamu nebo konfigurovat certifikáty SSL a předpony adres URL.</span><span class="sxs-lookup"><span data-stu-id="56445-196">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="56445-197">Uživatelské rozhraní je přesnější než ovladač http.sys Manager a zpřístupňuje několik dalších možností konfigurace, ale jinak poskytuje podobné funkce.</span><span class="sxs-lookup"><span data-stu-id="56445-197">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="56445-198">Nelze vytvořit nový seznam důvěryhodných certifikátů (CTL), ale můžete přiřadit existující.</span><span class="sxs-lookup"><span data-stu-id="56445-198">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="56445-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="56445-199">Next steps</span></span>

<span data-ttu-id="56445-200">Další informace naleznete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="56445-200">For more information, see the following resources:</span></span>

* [<span data-ttu-id="56445-201">Ukázková aplikace pro tohoto článku</span><span class="sxs-lookup"><span data-stu-id="56445-201">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="56445-202">Ovladač HTTP.sys zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="56445-202">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="56445-203">Hostování</span><span class="sxs-lookup"><span data-stu-id="56445-203">Hosting</span></span>](xref:fundamentals/hosting)
