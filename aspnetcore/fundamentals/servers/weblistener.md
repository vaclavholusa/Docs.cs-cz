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
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>WebListener webového serveru implementace v ASP.NET Core

Podle [tní Dykstra](https://github.com/tdykstra) a [Ross Jan](https://github.com/Tratcher)

> [!NOTE]
> Toto téma se vztahuje pouze na ASP.NET Core 1.x. V aplikaci ASP.NET 2.0 jádra, je název WebListener [HTTP.sys](httpsys.md).

Je WebListener [webového serveru pro ASP.NET Core](index.md) používající pouze v systému Windows. Je založen na [ovladač režimu jádra Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). WebListener je alternativa k [Kestrel](kestrel.md) který lze použít pro přímé připojení k Internetu, bez nutnosti spoléhat se ve službě IIS jako reverzní proxy server. Ve skutečnosti **WebListener nelze použít s služby IIS nebo IIS Express, protože není kompatibilní s [ASP.NET Core modulu](aspnet-core-module.md).**

I když WebListener byla vyvinuta pro ASP.NET Core, můžete použít přímo v libovolné aplikaci .NET Core nebo rozhraní .NET Framework prostřednictvím [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) balíček NuGet.

WebListener podporuje následující funkce:

- [Ověřování systému Windows](xref:security/authentication/windowsauth)
- Sdílení portů
- HTTPS s SNI
- HTTP/2 přes protokol TLS (Windows 10)
- Přímé souboru přenosu
- Ukládání odpovědí do mezipaměti
- Technologie WebSockets (Windows 8)

Podporované verze systému Windows:

- Windows 7 a Windows Server 2008 R2 a novější

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-weblistener"></a>Kdy použít WebListener

WebListener je užitečná pro nasazení, kde je nutné vystavit server přímo k Internetu bez použití služby IIS.

![Weblistener komunikuje přímo přes Internet](weblistener/_static/weblistener-to-internet.png)

Protože je založen na Http.Sys, WebListener nevyžaduje reverzní proxy server pro ochranu před útoky. Ovladač Http.Sys je Vyspělá technologie, která chrání před různé druhy útoky a poskytuje odolnost, zabezpečení a škálovatelnost plné webového serveru. Služba IIS spouští jako naslouchací proces protokolu HTTP na základě ovladače Http.Sys. 

WebListener je také vhodná pro interní nasazení, když potřebujete jedna z funkcí, které nabízí, nelze získat pomocí Kestrel.

![Weblistener komunikuje přímo s interní sítě](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>Jak používat WebListener

Zde je uveden přehled úloh nastavení pro hostitelský operační systém a aplikace ASP.NET Core.

### <a name="configure-windows-server"></a>Konfigurace Windows serveru

* Nainstalujte verzi rozhraní .NET, který vyžaduje vaše aplikace, jako například [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) nebo rozhraní .NET Framework 4.5.1.

* Preregister předpony adres URL a vytvořit vazbu na WebListener, nastavení certifikátů SSL

   Pokud nemáte preregister předpony adres URL v systému Windows, budete muset aplikaci spustit s oprávněním správce. Jedinou výjimkou je, pokud vytvoření vazby na localhost pomocí protokolu HTTP (nikoli HTTPS) se číslo portu větší než 1024; v takovém případě se vyžaduje oprávnění správce.

   Podrobnosti najdete v tématu [preregister předpony a konfigurace protokolu SSL](#preregister-url-prefixes-and-configure-ssl) dále v tomto článku.

* Otevřete porty brány firewall umožňující přenos k dosažení WebListener.

   Můžete použít netsh.exe nebo [rutiny prostředí PowerShell](https://technet.microsoft.com/library/jj554906).

Existují také [nastavení registru Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application"></a>Konfiguraci aplikace ASP.NET Core

* Nainstalujte balíček NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/). Tím se nainstaluje taky [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) jako závislost.

* Volání `UseWebListener` rozšiřující metody na [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) ve vaší `Main` metodu s uvedením všech WebListener [možnosti](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) a [nastavení](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) , které potřebujete , jak je znázorněno v následujícím příkladu:

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* Konfigurace adresy URL a portů pro naslouchání 

  Ve výchozím nastavení ASP.NET Core váže k `http://localhost:5000`. Konfigurace předpony adres URL a portů, můžete použít `UseURLs` metoda rozšíření `urls` argument příkazového řádku nebo konfigurační systém ASP.NET Core. Další informace najdete v tématu [hostitelský](../../fundamentals/hosting.md).

  Web používá naslouchací proces [formáty řetězců předponu Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx). Neexistují žádné požadavky formátu řetězec předpony, které jsou specifické pro WebListener.

  > [!NOTE]
  > Ujistěte se, zda jste zadali stejné předpona řetězce v `UseUrls` , preregister na serveru. 

* Ujistěte se, že vaše aplikace není nakonfigurována pro spuštění služby IIS nebo IIS Express.

  V sadě Visual Studio je výchozí profil spuštění pro službu IIS Express.  Pokud chcete spustit projekt jako konzolové aplikace budete muset ručně změnit vybraný profil, jak je znázorněno na následujícím snímku obrazovky.

  ![Vyberte profil aplikace konzoly](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>Jak používat WebListener mimo ASP.NET Core

* Nainstalujte [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) balíček NuGet.

* [Preregister předpony adres URL a vytvořit vazbu na WebListener, nastavení certifikátů SSL](#preregister-url-prefixes-and-configure-ssl) stejně jako pro použití v ASP.NET Core.

Existují také [nastavení registru Http.Sys](https://support.microsoft.com/kb/820129).


Zde je ukázka kódu, která demonstruje použití WebListener mimo ASP.NET Core:

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Preregister předpony adres URL a konfigurace protokolu SSL

Služba IIS a WebListener závisí na základní ovladač režimu jádra Http.Sys naslouchat požadavkům a počáteční zpracování. Ve službě IIS uživatelské rozhraní pro správu poskytuje relativně snadný způsob, jak nakonfigurovat vše, co. Ale pokud používáte WebListener budete muset nakonfigurovat Http.Sys sami. Integrované nástroje učinit je netsh.exe. 

Běžné úkoly, budete muset použít netsh.exe pro jsou rezervování předpony adres URL a přiřazení certifikáty SSL.

NetSh.exe není nástroj na snadno používat pro začátečníky. Následující příklad ukazuje úplné minimální počet potřebný pro rezervovat předpony adres URL pro porty 80 a 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Následující příklad ukazuje, jak přiřadit certifikát SSL:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

Zde je odkaz na oficiální dokumentaci:

* [Příkazy Netsh pro Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix řetězce](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Následující prostředky poskytují podrobné pokyny pro několik scénářů. Články, které odkazují na `HttpListener` platí jak pro `WebListener`, protože obě jsou založené na ovladače Http.Sys.

* [Postupy: Konfigurace portu certifikát protokolu SSL](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [Hostitelský a certifikát klienta na základě komunikaci pomocí protokolu HTTPS - HttpListener](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) to je blog třetí strany a je docela v minulosti, ale má stále užitečné informace.
* [Postupy: Použití HttpListener návod nebo Http Server nespravovaného kódu (C++) jako Server jednoduché SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) příliš jde starší blog s užitečné informace.
* [Jak mám nastavit .NET Core WebListener pomocí protokolu SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

Tady jsou některé nástroje třetích stran, které se dají použít jednodušší než netsh.exe příkazového řádku. Tyto jsou poskytované nebo schválené společností Microsoft. Nástroje spustit jako správce ve výchozím nastavení, protože netsh.exe samotné vyžaduje oprávnění správce.

* [ovladač HTTP.sys Manager](http://httpsysmanager.codeplex.com/) poskytuje uživatelské rozhraní pro výpis a konfiguraci certifikátů SSL a možnosti, předpony rezervace a seznamy důvěryhodných certifikátů. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) umožňuje seznamu nebo konfigurovat certifikáty SSL a předpony adres URL. Uživatelské rozhraní je přesnější než ovladač http.sys Manager a zpřístupňuje několik dalších možností konfigurace, ale jinak poskytuje podobné funkce. Nelze vytvořit nový seznam důvěryhodných certifikátů (CTL), ale můžete přiřadit existující.

Pro generování certifikátů SSL podepsaných svým držitelem, společnost Microsoft poskytuje nástroje pro příkazový řádek: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) a rutiny prostředí PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate). Existují také uživatelského rozhraní nástroje třetích stran, které bylo snazší pro vygenerování certifikáty podepsané svým držitelem SSL:

* [Nástroje SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [MakeCert uživatelského rozhraní](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>Další kroky

Další informace naleznete v následujících materiálech:

* [Ukázková aplikace pro tohoto článku](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [WebListener zdrojového kódu](https://github.com/aspnet/HttpSysServer/)
* [Hostování](../hosting.md)
