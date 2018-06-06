---
title: Ovladač HTTP.sys webového serveru implementace v ASP.NET Core
author: rick-anderson
description: Další informace o HTTP.sys, webový server pro ASP.NET Core v systému Windows. Založený na režimu jádra ovladač HTTP.sys, ovladač HTTP.sys je alternativa k Kestrel, který lze použít pro přímé připojení k Internetu bez služby IIS.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: dff798b19ad6d10a8ce93001ed4cebe732c54320
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729312"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Ovladač HTTP.sys webového serveru implementace v ASP.NET Core

Podle [tní Dykstra](https://github.com/tdykstra), [Jan Ross](https://github.com/Tratcher), a [Luke Latham](https://github.com/guardrex)

> [!NOTE]
> Toto téma se vztahuje na technologii ASP.NET Core 2.0 nebo novější. V dřívějších verzích ASP.NET Core, ovladač HTTP.sys jmenuje [WebListener](xref:fundamentals/servers/weblistener).

[Ovladač HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) je [webového serveru pro ASP.NET Core](xref:fundamentals/servers/index) , lze spustit pouze v systému Windows. Ovladač HTTP.sys je alternativa k [Kestrel](xref:fundamentals/servers/kestrel) a nabízí některé funkce, které neposkytuje Kestrel.

> [!IMPORTANT]
> Ovladač HTTP.sys není kompatibilní s [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) a nelze jej použít s služby IIS nebo IIS Express.

Ovladač HTTP.sys podporuje následující funkce:

* [Ověřování systému Windows](xref:security/authentication/windowsauth)
* Sdílení portů
* HTTPS s SNI
* HTTP/2 přes protokol TLS (Windows 10 nebo novější)
* Přímé souboru přenosu
* Ukládání odpovědí do mezipaměti
* Technologie WebSockets (Windows 8 nebo novější)

Podporované verze systému Windows:

* Windows 7 nebo novější
* Windows Server 2008 R2 nebo novější

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Kdy použít ovladače HTTP.sys

Ovladač HTTP.sys je užitečná pro nasazení kde:

* Je nutné vystavit server přímo k Internetu bez použití služby IIS.

  ![Ovladač HTTP.sys komunikuje přímo přes Internet](httpsys/_static/httpsys-to-internet.png)

* Interní nasazení vyžaduje funkci, není k dispozici v Kestrel, jako například [ověřování systému Windows](xref:security/authentication/windowsauth).

  ![Ovladač HTTP.sys komunikuje přímo s interní sítě](httpsys/_static/httpsys-to-internal.png)

Ovladač HTTP.sys je Vyspělá technologie, která chrání před mnoho typů útoků a poskytuje odolnost, zabezpečení a škálovatelnost plné webového serveru. Služba IIS spouští jako naslouchací proces protokolu HTTP na základě ovladače HTTP.sys. 

## <a name="how-to-use-httpsys"></a>Jak používat ovladač HTTP.sys

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a>Konfigurace aplikace ASP.NET Core používat ovladač HTTP.sys

1. Odkaz balíčku v souboru projektu není povinné, při použití [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 nebo vyšší). Pokud nepoužíváte `Microsoft.AspNetCore.App` metapackage, přidejte odkaz na balíček [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).

2. Volání [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) rozšíření metoda při vytváření webového hostitele, zadáte požadované [HTTP.sys možnosti](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions):

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   Další konfigurace ovladače HTTP.sys zpracovává prostřednictvím [nastavení registru](https://support.microsoft.com/kb/820129).

   **Ovladač HTTP.sys možnosti**

   | Vlastnost | Popis | Výchozí |
   | -------- | ----------- | :-----: |
   | [AllowSynchronousIO](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.allowsynchronousio) | Synchronní vstupu a výstupu mohou pro `HttpContext.Request.Body` a `HttpContext.Response.Body`. | `true` |
   | [Authentication.AllowAnonymous](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.allowanonymous) | Povolit anonymní žádosti. | `true` |
   | [Authentication.Schemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.schemes) | Zadejte schémat povolené ověřování. Může se změnit kdykoli před uvolnění naslouchací proces. Hodnoty jsou poskytovány [AuthenticationSchemes výčtu](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes): `Basic`, `Kerberos`, `Negotiate`, `None`, a `NTLM`. | `None` |
   | [EnableResponseCaching](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.enableresponsecaching) | Pokus o [režimu jádra](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) ukládání do mezipaměti pro odpovědi s oprávněné hlavičky. Odpověď nesmí obsahovat `Set-Cookie`, `Vary`, nebo `Pragma` hlavičky. Musí obsahovat `Cache-Control` záhlaví To je `public` a buď `shared-max-age` nebo `max-age` hodnotu, nebo `Expires` záhlaví. | `true` |
   | [MaxAccepts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxaccepts) | Maximální počet souběžných přijme. | 5 &times; [prostředí.<br> ProcessorCount](/dotnet/api/system.environment.processorcount) |
   | [maxConnections](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxconnections) | Maximální počet souběžných připojení tak, aby přijímal. Použití `-1` pro nekonečné. Použít `null` použít nastavení v registru počítače. | `null`<br>(bez omezení) |
   | [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) | Najdete v článku <a href="#maxrequestbodysize">MaxRequestBodySize</a> části. | 30000000 bajtů<br>(~28.6 MB) |
   | [RequestQueueLimit](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.requestqueuelimit) | Maximální počet požadavků, které lze zařadit do fronty. | 1000 |
   | [ThrowWriteExceptions](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.throwwriteexceptions) | Označuje Pokud zápisů text odpovědi, které odpojí nezdaří z důvodu klienta by měla vyvolat výjimky nebo dokončete běžným způsobem. | `false`<br>(dokončete běžným způsobem) |
   | [Časové limity](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts) | Vystavení ovladač HTTP.sys [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) konfigurace, který může být také nakonfigurována v registru. Rozhraní API odkazech na další informace o každém nastavení, včetně výchozí hodnoty:<ul><li>[Timeouts.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.drainentitybody) &ndash; čas povolené pro rozhraní API serveru HTTP na vyprázdnění obsahu entity na zachování připojení.</li><li>[Timeouts.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.entitybody) &ndash; čas povolené pro doručení v textu entity žádosti.</li><li>[Timeouts.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.headerwait) &ndash; čas povolené pro rozhraní API serveru HTTP k analýze hlavičky žádosti.</li><li>[Timeouts.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.idleconnection) &ndash; čas pro nečinné připojení povolená.</li><li>[Timeouts.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.minsendbytespersecond) &ndash; minimální míra pro odpověď v pro odesílání.</li><li>[Timeouts.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.requestqueue) &ndash; dobu povolenou pro požadavek na zůstat ve frontě požadavků, než aplikace převezme ho.</li></ul> |  |
   | [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) | Zadejte [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) pro registraci se ovladače HTTP.sys. Je velmi užitečné [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add), který se používá k přidání předpony do kolekce. To může být změněn kdykoli před uvolnění naslouchací proces. |  |

   <a name="maxrequestbodysize"></a>
   **MaxRequestBodySize**

   Maximální povolená velikost všech obsahu žádosti v bajtech. Pokud nastavíte hodnotu `null`, požadavek na maximální velikost obsahu neomezená. Tento limit nemá žádný vliv na upgradovaný připojení, které jsou vždy neomezená.

   Doporučené metody přepsat omezení v aplikaci ASP.NET MVC jádra pro jeden `IActionResult` je použití [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) atribut na metodu akce:

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   Pokud se aplikace pokusí Konfigurace limitu na vyžádání po spuštění aplikace čtení požadavku, je vyvolána výjimka. `IsReadOnly` Vlastnost lze použít pro oznámení, zda `MaxRequestBodySize` vlastnost je ve stavu jen pro čtení, což znamená, je příliš pozdě Konfigurace limitu.

   Pokud aplikace by měly přepsat [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) na žádost, použijte [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature):

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. Pokud pomocí sady Visual Studio, ujistěte se, že aplikace není nakonfigurovaná pro spuštění služby IIS nebo IIS Express.

   V sadě Visual Studio je výchozí profil spuštění pro službu IIS Express. Chcete-li spustit projekt jako konzolovou aplikaci, ručně změňte vybraný profil, jak je znázorněno na následujícím snímku obrazovky:

   ![Vyberte profil aplikace konzoly](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>Konfigurace Windows serveru

1. Pokud je aplikace [nasazení závislé na framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd), nainstalujte .NET Core a rozhraní .NET Framework (Pokud je aplikace k aplikaci .NET Core cílení na rozhraní .NET Framework).

   * **.NET core** &ndash; Pokud aplikace vyžaduje .NET Core, získání a spuštění instalačního programu .NET Core z [.NET všechny soubory ke stažení](https://www.microsoft.com/net/download/all).
   * **Rozhraní .NET framework** &ndash; Pokud aplikace vyžaduje rozhraní .NET Framework, najdete v části [rozhraní .NET Framework: Průvodce instalací](/dotnet/framework/install/) najít pokyny k instalaci. Instalace vyžaduje rozhraní .NET Framework. Instalační program pro nejnovější rozhraní .NET Framework naleznete na adrese [.NET všechny soubory ke stažení](https://www.microsoft.com/net/download/all).

2. Konfigurace adresy URL a portů pro aplikaci.

   Ve výchozím nastavení, ASP.NET Core váže k `http://localhost:5000`. Konfigurace předpony adres URL a portů, tyto možnosti, pomocí:

   * [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
   * `urls` Argument příkazového řádku
   * `ASPNETCORE_URLS` Proměnné prostředí
   * [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)

   Následující příklad kódu ukazuje, jak používat [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes):

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=11)]

   Výhodou `UrlPrefixes` se okamžitě vytvoří chybová zpráva pro nesprávně naformátovaný předpony.

   Nastavení v `UrlPrefixes` přepsat `UseUrls` / `urls` / `ASPNETCORE_URLS` nastavení. Proto výhoda `UseUrls`, `urls`a `ASPNETCORE_URLS` – proměnná prostředí je snadnější přepínat mezi Kestrel a ovladače HTTP.sys. Další informace o `UseUrls`, `urls`, a `ASPNETCORE_URLS`, najdete v článku [hostitele v ASP.NET Core](xref:fundamentals/host/index) tématu.

   Používá ovladače HTTP.sys [formáty řetězců UrlPrefix rozhraní API serveru HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

   > [!WARNING]
   > Vazby nejvyšší úrovně zástupný znak (`http://*:80/` a `http://+:80`) by měl **není** použít. Vazby nejvyšší úrovně zástupný znak můžete otevřít vaší aplikaci k ohrožení zabezpečení. To platí pro silné a slabé zástupné znaky. Použijte explicitní hostitele názvy místo zástupných znaků. Vazba subdomény zástupný znak (například `*.mysub.com`) nemá toto bezpečnostní riziko, pokud řízení celého nadřazené domény (Naproti tomu `*.com`, což je snadno napadnutelný). V tématu [rfc7230 části-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Další informace.

3. Preregister předpony adres URL pro svázání ovladače HTTP.sys a nastavit certifikáty x.509.

   Pokud v systému Windows nejsou preregistered předpony adres URL, spusťte aplikaci s oprávněními správce. Jedinou výjimkou je při vazbě na místního hostitele pomocí protokolu HTTP (nikoli HTTPS) se číslo portu větší než 1024. V takovém případě se vyžaduje oprávnění správce.

   1. Předdefinované nástroj pro konfiguraci HTTP.sys je *netsh.exe*. *Netsh.exe* se používá k rezervovat předpony adres URL a přiřaďte certifikáty X.509. Tento nástroj vyžaduje oprávnění správce.

      Následující příklad ukazuje příkazy tak, aby vyhradil předpony adres URL pro porty 80 a 443:

      ```console
      netsh http add urlacl url=http://+:80/ user=Users
      netsh http add urlacl url=https://+:443/ user=Users
      ```

      Následující příklad ukazuje, jak přiřadit certifikát X.509:

      ```console
      netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}"
      ```

      Referenční dokumentace pro *netsh.exe*:

      * [Příkazy Netsh pro Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
      * [UrlPrefix řetězce](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

   2. Certifikáty podepsané svým držitelem X.509, v případě potřeby vytvořte.

      [!INCLUDE [How to make an X.509 cert](../../includes/make-x509-cert.md)]

4. Otevřete porty brány firewall umožňující přenos k dosažení ovladače HTTP.sys. Použití *netsh.exe* nebo [rutiny prostředí PowerShell](https://technet.microsoft.com/library/jj554906).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy server a scénáře pro vyrovnávání zatížení

Pro aplikace hostované ovladače HTTP.sys, které se s požadavky z Internetu nebo podnikové síti můžou požadovat další konfigurace, při hostování za proxy servery a nástroje pro vyrovnávání zatížení. Další informace najdete v tématu [konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).

## <a name="additional-resources"></a>Další zdroje

* [Rozhraní API serveru HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [úložiště GitHub ASPNET/HttpSysServer (zdrojový kód)](https://github.com/aspnet/HttpSysServer/)
* [Hostitel v ASP.NET Core](xref:fundamentals/host/index)
