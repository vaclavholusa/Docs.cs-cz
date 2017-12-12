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
ms.openlocfilehash: d3f9eb4943ed62b674d6bb2ab1b275b0a3c02343
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Ovladač HTTP.sys webového serveru implementace v ASP.NET Core

Podle [tní Dykstra](https://github.com/tdykstra) a [Ross Jan](https://github.com/Tratcher)

> [!NOTE]
> Toto téma se vztahuje pouze na technologii ASP.NET 2.0 jádra a novější. V dřívějších verzích ASP.NET Core, ovladač HTTP.sys jmenuje [WebListener](xref:fundamentals/servers/weblistener).

Je ovladač HTTP.sys [webového serveru pro ASP.NET Core](index.md) používající pouze v systému Windows. Je založen na [ovladač režimu jádra Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). Ovladač HTTP.sys je alternativa k [Kestrel](kestrel.md) , nabízí některé funkce, které není Kestel. **Ovladač HTTP.sys nelze použít s služby IIS nebo IIS Express, protože není kompatibilní s [ASP.NET Core modulu](aspnet-core-module.md).**

Ovladač HTTP.sys podporuje následující funkce:

- [Ověřování systému Windows](xref:security/authentication/windowsauth)
- Sdílení portů
- HTTPS s SNI
- HTTP/2 přes protokol TLS (Windows 10)
- Přímé souboru přenosu
- Ukládání odpovědí do mezipaměti
- Technologie WebSockets (Windows 8)

Podporované verze systému Windows:

- Windows 7 a Windows Server 2008 R2 a novější

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Kdy použít ovladače HTTP.sys

Ovladač HTTP.sys je užitečná pro nasazení, kde je nutné vystavit server přímo k Internetu bez použití služby IIS.

![Ovladač HTTP.sys komunikuje přímo přes Internet](httpsys/_static/httpsys-to-internet.png)

Protože je založen na Http.Sys, ovladač HTTP.sys nevyžaduje reverzní proxy server pro ochranu před útoky. Ovladač Http.Sys je Vyspělá technologie, která chrání před různé druhy útoky a poskytuje odolnost, zabezpečení a škálovatelnost plné webového serveru. Služba IIS spouští jako naslouchací proces protokolu HTTP na základě ovladače Http.Sys. 

Ovladače HTTP.sys je vhodná pro interní nasazení, když potřebujete funkce není k dispozici v Kestrel, jako je například ověřování systému Windows.

![Ovladač HTTP.sys komunikuje přímo s interní sítě](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>Jak používat ovladač HTTP.sys

Zde je uveden přehled úloh nastavení pro hostitelský operační systém a aplikace ASP.NET Core.

### <a name="configure-windows-server"></a>Konfigurace Windows serveru

* Nainstalujte verzi rozhraní .NET, který vyžaduje vaše aplikace, jako například [.NET Core](https://www.microsoft.com/net/download/core) nebo [rozhraní .NET Framework](https://www.microsoft.com/net/download/framework).

* Preregister předpony adres URL pro svázání ovladače HTTP.sys a nastavení certifikátů SSL

   Pokud nemáte preregister předpony adres URL v systému Windows, budete muset aplikaci spustit s oprávněním správce. Jedinou výjimkou je, pokud vytvoření vazby na localhost pomocí protokolu HTTP (nikoli HTTPS) se číslo portu větší než 1024; v takovém případě se vyžaduje oprávnění správce.

   Podrobnosti najdete v tématu [preregister předpony a konfigurace protokolu SSL](#preregister-url-prefixes-and-configure-ssl) dále v tomto článku.

* Otevřete porty brány firewall umožňující přenos k dosažení ovladače HTTP.sys.

   Můžete použít *netsh.exe* nebo [rutiny prostředí PowerShell](https://technet.microsoft.com/library/jj554906).

Existují také [nastavení registru Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>Konfiguraci aplikace ASP.NET Core používat ovladač HTTP.sys

* Pokud používáte je nutná žádná instalace balíčku [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage. [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) balíček je součástí metapackage.

* Volání `UseHttpSys` rozšiřující metody na `WebHostBuilder` v vaše `Main` metoda, zadání žádné [HTTP.sys možnosti](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) , které potřebujete, jak je znázorněno v následujícím příkladu:

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>Konfigurace možností HTTP.sys

Zde jsou některé z ovladače HTTP.sys nastavení a omezení, která se dají konfigurovat.

**Maximální počet klientských připojení**

Maximální počet souběžných otevřete připojení TCP lze nastavit pro celou aplikaci s následujícím kódem v *Program.cs*:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

Maximální počet připojení je neomezená (null) ve výchozím nastavení.

**Velikost textu maximální požadavku**

Výchozí velikost těla maximální požadavku je 30,000,000 bajtů, což je přibližně 28.6 MB.

Doporučený způsob přepsat omezení v aplikaci ASP.NET MVC základní se má používat [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atribut na metodu akce:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Tady je příklad, který ukazuje, jak nakonfigurovat omezení pro celou aplikaci, každou žádost:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

Můžete přepsat nastavení konkrétního požadavku v *Startup.cs*:

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
Pokud se pokusíte nakonfigurovat limit na požadavek po spuštění aplikace čtení požadavku, je vyvolána výjimka. Je `IsReadOnly` vlastnost, která se dozvíte, pokud `MaxRequestBodySize` vlastnost je ve stavu jen pro čtení, tzn. je příliš pozdě Konfigurace limitu.

Informace o dalších možnostech HTTP.sys najdete v tématu [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). 

### <a name="configure-urls-and-ports-to-listen-on"></a>Konfigurace adresy URL a portů pro naslouchání 

Ve výchozím nastavení ASP.NET Core váže k `http://localhost:5000`. Konfigurace předpony adres URL a portů, můžete použít `UseUrls` metoda rozšíření `urls` argument příkazového řádku, proměnnou prostředí ASPNETCORE_URLS nebo `UrlPrefixes` vlastnost [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). Následující příklad kódu používá `UrlPrefixes`.

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

Výhodou `UrlPrefixes` je, že dostanete chybovou zprávu okamžitě pokud se pokusíte přidat předponu, která je špatně naformátovaný. Výhodou `UseUrls` (sdíleny s `urls` a ASPNETCORE_URLS) je, že můžete snadno přepínat mezi Kestrel a ovladače HTTP.sys.

Pokud používáte obě `UseUrls` (nebo `urls` nebo ASPNETCORE_URLS) a `UrlPrefixes`, nastavení v `UrlPrefixes` přepsat těm, které jsou v `UseUrls`. Další informace najdete v tématu [hostitelský](xref:fundamentals/hosting).

Používá ovladače HTTP.sys [formáty řetězců UrlPrefix rozhraní API serveru HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

> [!NOTE]
> Ujistěte se, zda jste zadali stejné předpona řetězce v `UseUrls` nebo `UrlPrefixes` , preregister na serveru. 

### <a name="dont-use-iis"></a>Nepoužívejte služby IIS

Ujistěte se, že vaše aplikace není nakonfigurována pro spuštění služby IIS nebo IIS Express.

V sadě Visual Studio je výchozí profil spuštění pro službu IIS Express. Chcete-li spustit projekt jako konzolové aplikace, ručně změňte vybraný profil, jak je znázorněno na následujícím snímku obrazovky.

![Vyberte profil aplikace konzoly](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Preregister předpony adres URL a konfigurace protokolu SSL

Služba IIS a ovladač HTTP.sys závisí na základní ovladač režimu jádra Http.Sys naslouchat požadavkům a počáteční zpracování. Ve službě IIS uživatelské rozhraní pro správu poskytuje relativně snadný způsob, jak nakonfigurovat vše, co. Však musíte nakonfigurovat Http.Sys sami. Integrované nástroje způsobem, který je *netsh.exe*. 

S *netsh.exe* můžete vyhradit předpony adres URL a přiřadit certifikáty SSL. Tento nástroj vyžaduje oprávnění správce.

Následující příklad ukazuje minimální počet potřebný pro rezervovat předpony adres URL pro porty 80 a 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Následující příklad ukazuje, jak přiřadit certifikát SSL:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

Tady je referenční dokumentaci k nástroji pro *netsh.exe*:

* [Příkazy Netsh pro Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix řetězce](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Následující prostředky poskytují podrobné pokyny pro několik scénářů. Články, které odkazují na HttpListener použít ovladače HTTP.sys, stejně jako obě jsou založené na ovladače Http.Sys.

* [Postupy: Konfigurace portu certifikát protokolu SSL](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [Hostitelský a certifikát klienta na základě komunikaci pomocí protokolu HTTPS - HttpListener](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) to je blog třetí strany a je docela v minulosti, ale má stále užitečné informace.
* [Postupy: Použití HttpListener návod nebo Http Server nespravovaného kódu (C++) jako Server jednoduché SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) příliš jde starší blog s užitečné informace.

Tady jsou některé nástroje třetích stran, které mohou být jednodušší než *netsh.exe* příkazového řádku. Tyto jsou poskytované nebo schválené společností Microsoft. Nástroje spustit jako správce ve výchozím nastavení, protože *netsh.exe* samotné vyžaduje oprávnění správce.

* [ovladač HTTP.sys Manager](http://httpsysmanager.codeplex.com/) poskytuje uživatelské rozhraní pro výpis a konfiguraci certifikátů SSL a možnosti, předpony rezervace a seznamy důvěryhodných certifikátů. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) umožňuje seznamu nebo konfigurovat certifikáty SSL a předpony adres URL. Uživatelské rozhraní je přesnější než ovladač http.sys Manager a zpřístupňuje několik dalších možností konfigurace, ale jinak poskytuje podobné funkce. Nelze vytvořit nový seznam důvěryhodných certifikátů (CTL), ale můžete přiřadit existující.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>Další kroky

Další informace naleznete v následujících materiálech:

* [Ukázková aplikace pro tohoto článku](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [Ovladač HTTP.sys zdrojového kódu](https://github.com/aspnet/HttpSysServer/)
* [Hostování](xref:fundamentals/hosting)
