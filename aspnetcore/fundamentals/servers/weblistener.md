---
title: "WebListener webového serveru implementace v ASP.NET Core"
author: rick-anderson
description: "Představuje WebListener, webový server pro ASP.NET Core v systému Windows. Postavená na ovladač Http.Sys v režimu jádra, WebListener je alternativa k Kestrel, který lze použít pro přímé připojení k Internetu bez služby IIS."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 5073a1663ec99a1b161092d74ab035ee9782becd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="929c9-104">WebListener webového serveru implementace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="929c9-104">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="929c9-105">Podle [tní Dykstra](https://github.com/tdykstra) a [Ross Jan](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="929c9-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="929c9-106">Toto téma se vztahuje pouze na ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="929c9-106">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="929c9-107">V aplikaci ASP.NET 2.0 jádra, je název WebListener [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="929c9-107">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="929c9-108">Je WebListener [webového serveru pro ASP.NET Core](index.md) používající pouze v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="929c9-108">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="929c9-109">Je založen na [ovladač režimu jádra Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="929c9-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="929c9-110">WebListener je alternativa k [Kestrel](kestrel.md) který lze použít pro přímé připojení k Internetu, bez nutnosti spoléhat se ve službě IIS jako reverzní proxy server.</span><span class="sxs-lookup"><span data-stu-id="929c9-110">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="929c9-111">Ve skutečnosti **WebListener nelze použít s služby IIS nebo IIS Express, protože není kompatibilní s [ASP.NET Core modulu](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="929c9-111">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="929c9-112">I když WebListener byla vyvinuta pro ASP.NET Core, můžete použít přímo v libovolné aplikaci .NET Core nebo rozhraní .NET Framework prostřednictvím [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="929c9-112">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="929c9-113">WebListener podporuje následující funkce:</span><span class="sxs-lookup"><span data-stu-id="929c9-113">WebListener supports the following features:</span></span>

- [<span data-ttu-id="929c9-114">Ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="929c9-114">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="929c9-115">Sdílení portů</span><span class="sxs-lookup"><span data-stu-id="929c9-115">Port sharing</span></span>
- <span data-ttu-id="929c9-116">HTTPS s SNI</span><span class="sxs-lookup"><span data-stu-id="929c9-116">HTTPS with SNI</span></span>
- <span data-ttu-id="929c9-117">HTTP/2 přes protokol TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="929c9-117">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="929c9-118">Přímé souboru přenosu</span><span class="sxs-lookup"><span data-stu-id="929c9-118">Direct file transmission</span></span>
- <span data-ttu-id="929c9-119">Ukládání odpovědí do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="929c9-119">Response caching</span></span>
- <span data-ttu-id="929c9-120">Technologie WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="929c9-120">WebSockets (Windows 8)</span></span>

<span data-ttu-id="929c9-121">Podporované verze systému Windows:</span><span class="sxs-lookup"><span data-stu-id="929c9-121">Supported Windows versions:</span></span>

- <span data-ttu-id="929c9-122">Windows 7 a Windows Server 2008 R2 a novější</span><span class="sxs-lookup"><span data-stu-id="929c9-122">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="929c9-123">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="929c9-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="929c9-124">Kdy použít WebListener</span><span class="sxs-lookup"><span data-stu-id="929c9-124">When to use WebListener</span></span>

<span data-ttu-id="929c9-125">WebListener je užitečná pro nasazení, kde je nutné vystavit server přímo k Internetu bez použití služby IIS.</span><span class="sxs-lookup"><span data-stu-id="929c9-125">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener komunikuje přímo přes Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="929c9-127">Protože je založen na Http.Sys, WebListener nevyžaduje reverzní proxy server pro ochranu před útoky.</span><span class="sxs-lookup"><span data-stu-id="929c9-127">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="929c9-128">Ovladač Http.Sys je Vyspělá technologie, která chrání před různé druhy útoky a poskytuje odolnost, zabezpečení a škálovatelnost plné webového serveru.</span><span class="sxs-lookup"><span data-stu-id="929c9-128">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="929c9-129">Služba IIS spouští jako naslouchací proces protokolu HTTP na základě ovladače Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="929c9-129">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="929c9-130">WebListener je také vhodná pro interní nasazení, když potřebujete jedna z funkcí, které nabízí, nelze získat pomocí Kestrel.</span><span class="sxs-lookup"><span data-stu-id="929c9-130">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener komunikuje přímo s interní sítě](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="929c9-132">Jak používat WebListener</span><span class="sxs-lookup"><span data-stu-id="929c9-132">How to use WebListener</span></span>

<span data-ttu-id="929c9-133">Zde je uveden přehled úloh nastavení pro hostitelský operační systém a aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="929c9-133">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="929c9-134">Konfigurace Windows serveru</span><span class="sxs-lookup"><span data-stu-id="929c9-134">Configure Windows Server</span></span>

* <span data-ttu-id="929c9-135">Nainstalujte verzi rozhraní .NET, který vyžaduje vaše aplikace, jako například [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) nebo rozhraní .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="929c9-135">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="929c9-136">Preregister předpony adres URL a vytvořit vazbu na WebListener, nastavení certifikátů SSL</span><span class="sxs-lookup"><span data-stu-id="929c9-136">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="929c9-137">Pokud nemáte preregister předpony adres URL v systému Windows, budete muset aplikaci spustit s oprávněním správce.</span><span class="sxs-lookup"><span data-stu-id="929c9-137">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="929c9-138">Jedinou výjimkou je, pokud vytvoření vazby na localhost pomocí protokolu HTTP (nikoli HTTPS) se číslo portu větší než 1024; v takovém případě se vyžaduje oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="929c9-138">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="929c9-139">Podrobnosti najdete v tématu [preregister předpony a konfigurace protokolu SSL](#preregister-url-prefixes-and-configure-ssl) dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="929c9-139">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="929c9-140">Otevřete porty brány firewall umožňující přenos k dosažení WebListener.</span><span class="sxs-lookup"><span data-stu-id="929c9-140">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="929c9-141">Můžete použít netsh.exe nebo [rutiny prostředí PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="929c9-141">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="929c9-142">Existují také [nastavení registru Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="929c9-142">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="929c9-143">Konfiguraci aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="929c9-143">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="929c9-144">Nainstalujte balíček NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="929c9-144">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="929c9-145">Tím se nainstaluje taky [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) jako závislost.</span><span class="sxs-lookup"><span data-stu-id="929c9-145">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="929c9-146">Volání `UseWebListener` rozšiřující metody na [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) ve vaší `Main` metodu s uvedením všech WebListener [možnosti](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) a [nastavení](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) , které potřebujete , jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="929c9-146">Call the `UseWebListener` extension method on [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="929c9-147">Konfigurace adresy URL a portů pro naslouchání</span><span class="sxs-lookup"><span data-stu-id="929c9-147">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="929c9-148">Ve výchozím nastavení ASP.NET Core váže k `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="929c9-148">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="929c9-149">Konfigurace předpony adres URL a portů, můžete použít `UseURLs` metoda rozšíření `urls` argument příkazového řádku nebo konfigurační systém ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="929c9-149">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="929c9-150">Další informace najdete v tématu [hostitelský](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="929c9-150">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="929c9-151">Web používá naslouchací proces [formáty řetězců předponu Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="929c9-151">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="929c9-152">Neexistují žádné požadavky formátu řetězec předpony, které jsou specifické pro WebListener.</span><span class="sxs-lookup"><span data-stu-id="929c9-152">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!NOTE]
  > <span data-ttu-id="929c9-153">Ujistěte se, zda jste zadali stejné předpona řetězce v `UseUrls` , preregister na serveru.</span><span class="sxs-lookup"><span data-stu-id="929c9-153">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="929c9-154">Ujistěte se, že vaše aplikace není nakonfigurována pro spuštění služby IIS nebo IIS Express.</span><span class="sxs-lookup"><span data-stu-id="929c9-154">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="929c9-155">V sadě Visual Studio je výchozí profil spuštění pro službu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="929c9-155">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="929c9-156">Pokud chcete spustit projekt jako konzolové aplikace budete muset ručně změnit vybraný profil, jak je znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="929c9-156">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Vyberte profil aplikace konzoly](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="929c9-158">Jak používat WebListener mimo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="929c9-158">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="929c9-159">Nainstalujte [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="929c9-159">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="929c9-160">[Preregister předpony adres URL a vytvořit vazbu na WebListener, nastavení certifikátů SSL](#preregister-url-prefixes-and-configure-ssl) stejně jako pro použití v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="929c9-160">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="929c9-161">Existují také [nastavení registru Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="929c9-161">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="929c9-162">Zde je ukázka kódu, která demonstruje použití WebListener mimo ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="929c9-162">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="929c9-163">Preregister předpony adres URL a konfigurace protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="929c9-163">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="929c9-164">Služba IIS a WebListener závisí na základní ovladač režimu jádra Http.Sys naslouchat požadavkům a počáteční zpracování.</span><span class="sxs-lookup"><span data-stu-id="929c9-164">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="929c9-165">Ve službě IIS uživatelské rozhraní pro správu poskytuje relativně snadný způsob, jak nakonfigurovat vše, co.</span><span class="sxs-lookup"><span data-stu-id="929c9-165">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="929c9-166">Ale pokud používáte WebListener budete muset nakonfigurovat Http.Sys sami.</span><span class="sxs-lookup"><span data-stu-id="929c9-166">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="929c9-167">Předdefinované nástroj pro to, který je netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="929c9-167">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="929c9-168">Běžné úkoly, budete muset použít netsh.exe pro jsou rezervování předpony adres URL a přiřazení certifikáty SSL.</span><span class="sxs-lookup"><span data-stu-id="929c9-168">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="929c9-169">NetSh.exe není nástroj na snadno používat pro začátečníky.</span><span class="sxs-lookup"><span data-stu-id="929c9-169">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="929c9-170">Následující příklad ukazuje úplné minimální počet potřebný pro rezervovat předpony adres URL pro porty 80 a 443:</span><span class="sxs-lookup"><span data-stu-id="929c9-170">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="929c9-171">Následující příklad ukazuje, jak přiřadit certifikát SSL:</span><span class="sxs-lookup"><span data-stu-id="929c9-171">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="929c9-172">Zde je odkaz na oficiální dokumentaci:</span><span class="sxs-lookup"><span data-stu-id="929c9-172">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="929c9-173">Příkazy Netsh pro Hypertext Transfer Protocol (HTTP)</span><span class="sxs-lookup"><span data-stu-id="929c9-173">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="929c9-174">UrlPrefix řetězce</span><span class="sxs-lookup"><span data-stu-id="929c9-174">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="929c9-175">Následující prostředky poskytují podrobné pokyny pro několik scénářů.</span><span class="sxs-lookup"><span data-stu-id="929c9-175">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="929c9-176">Články, které odkazují na `HttpListener` platí jak pro `WebListener`, protože obě jsou založené na ovladače Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="929c9-176">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="929c9-177">Postupy: Konfigurace portu s certifikátem SSL</span><span class="sxs-lookup"><span data-stu-id="929c9-177">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="929c9-178">[Hostitelský a certifikát klienta na základě komunikaci pomocí protokolu HTTPS - HttpListener](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) to je blog třetí strany a je docela v minulosti, ale má stále užitečné informace.</span><span class="sxs-lookup"><span data-stu-id="929c9-178">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="929c9-179">[Postupy: Použití HttpListener návod nebo Http Server nespravovaného kódu (C++) jako Server jednoduché SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) příliš jde starší blog s užitečné informace.</span><span class="sxs-lookup"><span data-stu-id="929c9-179">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="929c9-180">Jak mám nastavit .NET Core WebListener pomocí protokolu SSL?</span><span class="sxs-lookup"><span data-stu-id="929c9-180">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="929c9-181">Tady jsou některé nástroje třetích stran, které se dají použít jednodušší než netsh.exe příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="929c9-181">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="929c9-182">Tyto jsou poskytované nebo schválené společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="929c9-182">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="929c9-183">Nástroje spustit jako správce ve výchozím nastavení, protože netsh.exe samotné vyžaduje oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="929c9-183">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="929c9-184">[ovladač HTTP.sys Manager](http://httpsysmanager.codeplex.com/) poskytuje uživatelské rozhraní pro výpis a konfiguraci certifikátů SSL a možnosti, předpony rezervace a seznamy důvěryhodných certifikátů.</span><span class="sxs-lookup"><span data-stu-id="929c9-184">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="929c9-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) umožňuje seznamu nebo konfigurovat certifikáty SSL a předpony adres URL.</span><span class="sxs-lookup"><span data-stu-id="929c9-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="929c9-186">Uživatelské rozhraní je přesnější než ovladač http.sys Manager a zpřístupňuje několik dalších možností konfigurace, ale jinak poskytuje podobné funkce.</span><span class="sxs-lookup"><span data-stu-id="929c9-186">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="929c9-187">Nelze vytvořit nový seznam důvěryhodných certifikátů (CTL), ale můžete přiřadit existující.</span><span class="sxs-lookup"><span data-stu-id="929c9-187">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="929c9-188">Pro generování certifikátů SSL podepsaných svým držitelem, společnost Microsoft poskytuje nástroje pro příkazový řádek: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) a rutiny prostředí PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="929c9-188">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="929c9-189">Existují také uživatelského rozhraní nástroje třetích stran, které bylo snazší pro vygenerování certifikáty podepsané svým držitelem SSL:</span><span class="sxs-lookup"><span data-stu-id="929c9-189">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="929c9-190">SelfCert</span><span class="sxs-lookup"><span data-stu-id="929c9-190">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="929c9-191">MakeCert uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="929c9-191">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="929c9-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="929c9-192">Next steps</span></span>

<span data-ttu-id="929c9-193">Další informace naleznete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="929c9-193">For more information, see the following resources:</span></span>

* [<span data-ttu-id="929c9-194">Ukázková aplikace pro tohoto článku</span><span class="sxs-lookup"><span data-stu-id="929c9-194">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="929c9-195">WebListener zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="929c9-195">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="929c9-196">Hostování</span><span class="sxs-lookup"><span data-stu-id="929c9-196">Hosting</span></span>](../hosting.md)
