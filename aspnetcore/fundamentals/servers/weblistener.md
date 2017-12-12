---
title: "WebListener webového serveru implementace v ASP.NET Core"
author: rick-anderson
description: "Představuje WebListener, webový server pro ASP.NET Core v systému Windows. Postavená na ovladač Http.Sys v režimu jádra, WebListener je alternativa k Kestrel, který lze použít pro přímé připojení k Internetu bez služby IIS."
keywords: "ASP.NET Core, WebListener, HttpListener, předpony adres url, protokol SSL"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: f1abb3558546cd907c78b44d9353d9c9f1f5aff1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="99de3-105">WebListener webového serveru implementace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="99de3-105">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="99de3-106">Podle [tní Dykstra](https://github.com/tdykstra) a [Ross Jan](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="99de3-106">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="99de3-107">Toto téma se vztahuje pouze na ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="99de3-107">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="99de3-108">V aplikaci ASP.NET 2.0 jádra, je název WebListener [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="99de3-108">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="99de3-109">Je WebListener [webového serveru pro ASP.NET Core](index.md) používající pouze v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="99de3-109">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="99de3-110">Je založen na [ovladač režimu jádra Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="99de3-110">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="99de3-111">WebListener je alternativa k [Kestrel](kestrel.md) který lze použít pro přímé připojení k Internetu, bez nutnosti spoléhat se ve službě IIS jako reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="99de3-111">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="99de3-112">Ve skutečnosti **WebListener nelze použít s služby IIS nebo IIS Express, protože není kompatibilní s [ASP.NET Core modulu](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="99de3-112">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="99de3-113">I když WebListener byla vyvinuta pro ASP.NET Core, můžete použít přímo v libovolné aplikaci .NET Core nebo rozhraní .NET Framework prostřednictvím [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="99de3-113">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="99de3-114">WebListener podporuje následující funkce:</span><span class="sxs-lookup"><span data-stu-id="99de3-114">WebListener supports the following features:</span></span>

- [<span data-ttu-id="99de3-115">Ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="99de3-115">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="99de3-116">Sdílení portů</span><span class="sxs-lookup"><span data-stu-id="99de3-116">Port sharing</span></span>
- <span data-ttu-id="99de3-117">HTTPS s SNI</span><span class="sxs-lookup"><span data-stu-id="99de3-117">HTTPS with SNI</span></span>
- <span data-ttu-id="99de3-118">HTTP/2 přes protokol TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="99de3-118">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="99de3-119">Přímé souboru přenosu</span><span class="sxs-lookup"><span data-stu-id="99de3-119">Direct file transmission</span></span>
- <span data-ttu-id="99de3-120">Ukládání odpovědí do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="99de3-120">Response caching</span></span>
- <span data-ttu-id="99de3-121">Technologie WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="99de3-121">WebSockets (Windows 8)</span></span>

<span data-ttu-id="99de3-122">Podporované verze systému Windows:</span><span class="sxs-lookup"><span data-stu-id="99de3-122">Supported Windows versions:</span></span>

- <span data-ttu-id="99de3-123">Windows 7 a Windows Server 2008 R2 a novější</span><span class="sxs-lookup"><span data-stu-id="99de3-123">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="99de3-124">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="99de3-124">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="99de3-125">Kdy použít WebListener</span><span class="sxs-lookup"><span data-stu-id="99de3-125">When to use WebListener</span></span>

<span data-ttu-id="99de3-126">WebListener je užitečná pro nasazení, kde je nutné vystavit server přímo k Internetu bez použití služby IIS.</span><span class="sxs-lookup"><span data-stu-id="99de3-126">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener komunikuje přímo přes Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="99de3-128">Protože je založen na Http.Sys, WebListener nevyžaduje reverzní proxy server pro ochranu před útoky.</span><span class="sxs-lookup"><span data-stu-id="99de3-128">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="99de3-129">Ovladač Http.Sys je Vyspělá technologie, která chrání před různé druhy útoky a poskytuje odolnost, zabezpečení a škálovatelnost plné webového serveru.</span><span class="sxs-lookup"><span data-stu-id="99de3-129">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="99de3-130">Služba IIS spouští jako naslouchací proces protokolu HTTP na základě ovladače Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="99de3-130">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="99de3-131">WebListener je také vhodná pro interní nasazení, když potřebujete jedna z funkcí, které nabízí, nelze získat pomocí Kestrel.</span><span class="sxs-lookup"><span data-stu-id="99de3-131">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener komunikuje přímo s interní sítě](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="99de3-133">Jak používat WebListener</span><span class="sxs-lookup"><span data-stu-id="99de3-133">How to use WebListener</span></span>

<span data-ttu-id="99de3-134">Zde je uveden přehled úloh nastavení pro hostitelský operační systém a aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="99de3-134">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="99de3-135">Konfigurace Windows serveru</span><span class="sxs-lookup"><span data-stu-id="99de3-135">Configure Windows Server</span></span>

* <span data-ttu-id="99de3-136">Nainstalujte verzi rozhraní .NET, který vyžaduje vaše aplikace, jako například [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) nebo rozhraní .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="99de3-136">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="99de3-137">Preregister předpony adres URL a vytvořit vazbu na WebListener, nastavení certifikátů SSL</span><span class="sxs-lookup"><span data-stu-id="99de3-137">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="99de3-138">Pokud nemáte preregister předpony adres URL v systému Windows, budete muset aplikaci spustit s oprávněním správce.</span><span class="sxs-lookup"><span data-stu-id="99de3-138">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="99de3-139">Jedinou výjimkou je, pokud vytvoření vazby na localhost pomocí protokolu HTTP (nikoli HTTPS) se číslo portu větší než 1024; v takovém případě se vyžaduje oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="99de3-139">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="99de3-140">Podrobnosti najdete v tématu [preregister předpony a konfigurace protokolu SSL](#preregister-url-prefixes-and-configure-ssl) dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="99de3-140">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="99de3-141">Otevřete porty brány firewall umožňující přenos k dosažení WebListener.</span><span class="sxs-lookup"><span data-stu-id="99de3-141">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="99de3-142">Můžete použít netsh.exe nebo [rutiny prostředí PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="99de3-142">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="99de3-143">Existují také [nastavení registru Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="99de3-143">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="99de3-144">Konfiguraci aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="99de3-144">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="99de3-145">Nainstalujte balíček NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="99de3-145">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="99de3-146">Tím se nainstaluje taky [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) jako závislost.</span><span class="sxs-lookup"><span data-stu-id="99de3-146">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="99de3-147">Volání `UseWebListener` rozšiřující metody na [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) ve vaší `Main` metodu s uvedením všech WebListener [možnosti](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) a [nastavení](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) , které potřebujete , jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="99de3-147">Call the `UseWebListener` extension method on [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="99de3-148">Konfigurace adresy URL a portů pro naslouchání</span><span class="sxs-lookup"><span data-stu-id="99de3-148">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="99de3-149">Ve výchozím nastavení ASP.NET Core váže k `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="99de3-149">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="99de3-150">Konfigurace předpony adres URL a portů, můžete použít `UseURLs` metoda rozšíření `urls` argument příkazového řádku nebo konfigurační systém ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="99de3-150">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="99de3-151">Další informace najdete v tématu [hostitelský](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="99de3-151">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="99de3-152">Web používá naslouchací proces [formáty řetězců předponu Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="99de3-152">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="99de3-153">Neexistují žádné požadavky formátu řetězec předpony, které jsou specifické pro WebListener.</span><span class="sxs-lookup"><span data-stu-id="99de3-153">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!NOTE]
  > <span data-ttu-id="99de3-154">Ujistěte se, zda jste zadali stejné předpona řetězce v `UseUrls` , preregister na serveru.</span><span class="sxs-lookup"><span data-stu-id="99de3-154">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="99de3-155">Ujistěte se, že vaše aplikace není nakonfigurována pro spuštění služby IIS nebo IIS Express.</span><span class="sxs-lookup"><span data-stu-id="99de3-155">Make sure your application is not configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="99de3-156">V sadě Visual Studio je výchozí profil spuštění pro službu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="99de3-156">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="99de3-157">Pokud chcete spustit projekt jako konzolové aplikace budete muset ručně změnit vybraný profil, jak je znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="99de3-157">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Vyberte profil aplikace konzoly](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="99de3-159">Jak používat WebListener mimo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="99de3-159">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="99de3-160">Nainstalujte [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="99de3-160">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="99de3-161">[Preregister předpony adres URL a vytvořit vazbu na WebListener, nastavení certifikátů SSL](#preregister-url-prefixes-and-configure-ssl) stejně jako pro použití v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="99de3-161">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="99de3-162">Existují také [nastavení registru Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="99de3-162">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="99de3-163">Zde je ukázka kódu, která demonstruje použití WebListener mimo ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="99de3-163">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="99de3-164">Preregister předpony adres URL a konfigurace protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="99de3-164">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="99de3-165">Služba IIS a WebListener závisí na základní ovladač režimu jádra Http.Sys naslouchat požadavkům a počáteční zpracování.</span><span class="sxs-lookup"><span data-stu-id="99de3-165">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="99de3-166">Ve službě IIS uživatelské rozhraní pro správu poskytuje relativně snadný způsob, jak nakonfigurovat vše, co.</span><span class="sxs-lookup"><span data-stu-id="99de3-166">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="99de3-167">Ale pokud používáte WebListener budete muset nakonfigurovat Http.Sys sami.</span><span class="sxs-lookup"><span data-stu-id="99de3-167">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="99de3-168">Integrované nástroje učinit je netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="99de3-168">The built-in tool for doing that is netsh.exe.</span></span> 

<span data-ttu-id="99de3-169">Běžné úkoly, budete muset použít netsh.exe pro jsou rezervování předpony adres URL a přiřazení certifikáty SSL.</span><span class="sxs-lookup"><span data-stu-id="99de3-169">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="99de3-170">NetSh.exe není nástroj na snadno používat pro začátečníky.</span><span class="sxs-lookup"><span data-stu-id="99de3-170">NetSh.exe is not an easy tool to use for beginners.</span></span> <span data-ttu-id="99de3-171">Následující příklad ukazuje úplné minimální počet potřebný pro rezervovat předpony adres URL pro porty 80 a 443:</span><span class="sxs-lookup"><span data-stu-id="99de3-171">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="99de3-172">Následující příklad ukazuje, jak přiřadit certifikát SSL:</span><span class="sxs-lookup"><span data-stu-id="99de3-172">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="99de3-173">Zde je odkaz na oficiální dokumentaci:</span><span class="sxs-lookup"><span data-stu-id="99de3-173">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="99de3-174">Příkazy Netsh pro Hypertext Transfer Protocol (HTTP)</span><span class="sxs-lookup"><span data-stu-id="99de3-174">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="99de3-175">UrlPrefix řetězce</span><span class="sxs-lookup"><span data-stu-id="99de3-175">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="99de3-176">Následující prostředky poskytují podrobné pokyny pro několik scénářů.</span><span class="sxs-lookup"><span data-stu-id="99de3-176">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="99de3-177">Články, které odkazují na `HttpListener` platí jak pro `WebListener`, protože obě jsou založené na ovladače Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="99de3-177">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="99de3-178">Postupy: Konfigurace portu certifikát protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="99de3-178">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="99de3-179">[Hostitelský a certifikát klienta na základě komunikaci pomocí protokolu HTTPS - HttpListener](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) to je blog třetí strany a je docela v minulosti, ale má stále užitečné informace.</span><span class="sxs-lookup"><span data-stu-id="99de3-179">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="99de3-180">[Postupy: Použití HttpListener návod nebo Http Server nespravovaného kódu (C++) jako Server jednoduché SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) příliš jde starší blog s užitečné informace.</span><span class="sxs-lookup"><span data-stu-id="99de3-180">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="99de3-181">Jak mám nastavit .NET Core WebListener pomocí protokolu SSL?</span><span class="sxs-lookup"><span data-stu-id="99de3-181">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="99de3-182">Tady jsou některé nástroje třetích stran, které se dají použít jednodušší než netsh.exe příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="99de3-182">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="99de3-183">Tyto jsou poskytované nebo schválené společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="99de3-183">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="99de3-184">Nástroje spustit jako správce ve výchozím nastavení, protože netsh.exe samotné vyžaduje oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="99de3-184">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="99de3-185">[ovladač HTTP.sys Manager](http://httpsysmanager.codeplex.com/) poskytuje uživatelské rozhraní pro výpis a konfiguraci certifikátů SSL a možnosti, předpony rezervace a seznamy důvěryhodných certifikátů.</span><span class="sxs-lookup"><span data-stu-id="99de3-185">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="99de3-186">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) umožňuje seznamu nebo konfigurovat certifikáty SSL a předpony adres URL.</span><span class="sxs-lookup"><span data-stu-id="99de3-186">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="99de3-187">Uživatelské rozhraní je přesnější než ovladač http.sys Manager a zpřístupňuje několik dalších možností konfigurace, ale jinak poskytuje podobné funkce.</span><span class="sxs-lookup"><span data-stu-id="99de3-187">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="99de3-188">Nelze vytvořit nový seznam důvěryhodných certifikátů (CTL), ale můžete přiřadit existující.</span><span class="sxs-lookup"><span data-stu-id="99de3-188">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="99de3-189">Pro generování certifikátů SSL podepsaných svým držitelem, společnost Microsoft poskytuje nástroje pro příkazový řádek: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) a rutiny prostředí PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="99de3-189">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="99de3-190">Existují také uživatelského rozhraní nástroje třetích stran, které bylo snazší pro vygenerování certifikáty podepsané svým držitelem SSL:</span><span class="sxs-lookup"><span data-stu-id="99de3-190">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="99de3-191">Nástroje SelfCert</span><span class="sxs-lookup"><span data-stu-id="99de3-191">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="99de3-192">MakeCert uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="99de3-192">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="99de3-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="99de3-193">Next steps</span></span>

<span data-ttu-id="99de3-194">Další informace naleznete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="99de3-194">For more information, see the following resources:</span></span>

* [<span data-ttu-id="99de3-195">Ukázková aplikace pro tohoto článku</span><span class="sxs-lookup"><span data-stu-id="99de3-195">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="99de3-196">WebListener zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="99de3-196">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="99de3-197">Hostování</span><span class="sxs-lookup"><span data-stu-id="99de3-197">Hosting</span></span>](../hosting.md)
