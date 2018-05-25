---
title: Implementace webového serveru v ASP.NET Core
author: rick-anderson
description: Zjištění webové servery Kestrel a ovladače HTTP.sys pro ASP.NET Core. Zjistěte, jak vybrat server a kdy použít reverzní proxy server.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/index
ms.openlocfilehash: c9ed385208df083f631174c7071ca31ed2114350
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/24/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a>Implementace webového serveru v ASP.NET Core

Podle [tní Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), a [Ross Jan](https://github.com/Tratcher)

Aplikace ASP.NET Core spustí se v procesu implementaci serveru HTTP. Implementace server naslouchá pro požadavky protokolu HTTP a zobrazí je v aplikaci jako sady [žádosti o funkce](xref:fundamentals/request-features) složený do [HttpContext](/dotnet/api/system.web.httpcontext).

ASP.NET Core dodává dvě implementace serveru:

* [Kestrel](xref:fundamentals/servers/kestrel) je výchozí, server HTTP a platformy pro ASP.NET Core.
* [Ovladač HTTP.sys](xref:fundamentals/servers/httpsys) je na základě protokolu HTTP pouze pro systém Windows server [ovladač HTTP.sys jádra a rozhraní API serveru HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). (Ovladač HTTP.sys nazývá [WebListener](xref:fundamentals/servers/weblistener) v ASP.NET Core 1.x.)

## <a name="kestrel"></a>kestrel

Kestrel je součástí šablony projektů ASP.NET Core výchozí webový server.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Kestrel můžete použít samostatně nebo se *reverzní proxy server*, jako jsou například služby IIS, Nginx nebo Apache. Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování.

![Kestrel komunikuje přímo s Internetu bez reverzní proxy server](kestrel/_static/kestrel-to-internet2.png)

![Kestrel nepřímo komunikuje v Internetu přes reverzní proxy server, například služby IIS, Nginx nebo Apache](kestrel/_static/kestrel-to-internet.png)

Buď konfiguraci&mdash;s nebo bez reverzní proxy server&mdash;je platný a podporované konfigurace hostování pro technologii ASP.NET Core 2.0 nebo novější. Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Aplikaci jenom přijímá požadavky od k interní síti, lze nastavit Kestrel samostatně.

![Kestrel komunikuje přímo s interní sítě](kestrel/_static/kestrel-to-internal.png)

Pokud je aplikace přístup k Internetu, Kestrel musí používat službu IIS, Nginx nebo Apache jako *reverzní proxy server*. Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování, jak je znázorněno v následujícím diagramu:

![Kestrel nepřímo komunikuje v Internetu přes reverzní proxy server, například služby IIS, Nginx nebo Apache](kestrel/_static/kestrel-to-internet.png)

Nejdůležitější důvod pro používání reverzní proxy server pro nasazení okraj (vystavený pro přenosy z Internetu) je zabezpečení. Verze 1.x Kestrel nemají funkce důležité zabezpečení se chránit před útoky ze sítě Internet. To zahrnuje, ale není omezen na, příslušné vypršení časových limitů, omezení velikosti požadavků a omezení počtu souběžných připojení.

Další informace najdete v tématu [použití Kestrel s reverzní proxy server](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

---

Služba IIS, Nginx a Apache nejde použít bez Kestrel nebo [implementace vlastního serveru](#custom-servers). ASP.NET Core byl navržen ke spuštění ve svém vlastním procesu, takže může chovat konzistentně napříč platformami. Služba IIS, Nginx a Apache určují vlastní při spuštění procedury a prostředí. Tyto technologie serveru přímo, ASP.NET Core by chtít použít přizpůsobit požadavkům každého serveru. Pomocí implementaci serveru web, jako je například Kestrel, ASP.NET Core má kontrolu nad spuštění procesu a prostředí při hostované na jiný server technologie.

### <a name="iis-with-kestrel"></a>IIS s Kestrel

Při použití [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) nebo [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) jako reverzní proxy server pro ASP.NET Core, ASP.NET Core aplikace běží v procesu odděleně od pracovní proces služby IIS. V procesu služby IIS [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) koordinuje relace reverzní proxy server. Primární funkce modulu jádra ASP.NET se spustí aplikaci ASP.NET Core, restartujte aplikaci, když ho dojde k chybě a přesměrování provozu HTTP do aplikace. Další informace najdete v tématu [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module). 

### <a name="nginx-with-kestrel"></a>Nginx s Kestrel

Informace o tom, jak používat Nginx v systému Linux jako reverzní proxy server pro Kestrel najdete v tématu [hostitele v systému Linux s Nginx](xref:host-and-deploy/linux-nginx).

### <a name="apache-with-kestrel"></a>Apache s Kestrel

Informace o tom, jak používat Apache v systému Linux jako reverzní proxy server pro Kestrel najdete v tématu [hostitele v systému Linux s Apache](xref:host-and-deploy/linux-apache).

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Pokud aplikace ASP.NET Core spouštějí v systému Windows, ovladač HTTP.sys je alternativa k Kestrel. Kestrel obecně doporučujeme pro nejlepší výkon. Ovladač HTTP.sys lze použít ve scénářích, kde aplikace je přístup k Internetu a podporuje požadované funkce, ale není Kestrel ovladače HTTP.sys. Informace o funkcích ovladač HTTP.sys najdete v tématu [HTTP.sys](xref:fundamentals/servers/httpsys).

![Ovladač HTTP.sys komunikuje přímo přes Internet](httpsys/_static/httpsys-to-internet.png)

Ovladač HTTP.sys mohou sloužit také pro aplikace, které jsou přístupné pouze pro interní síť. 

![Ovladač HTTP.sys komunikuje přímo s interní sítě](httpsys/_static/httpsys-to-internal.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Ovladač HTTP.sys jmenuje [WebListener](xref:fundamentals/servers/weblistener) v ASP.NET Core 1.x. Pokud aplikace ASP.NET Core spouštějí v systému Windows, je WebListener alternativu pro scénáře, kde není k dispozici pro hostitele aplikace služby IIS.

![Weblistener komunikuje přímo přes Internet](weblistener/_static/weblistener-to-internet.png)

WebListener také jde použít místo Kestrel pro aplikace, které jsou přístupné pouze pro interní síť, v případě potřeby funkce se podporuje, ale ne Kestrel WebListener. Informace o funkcích WebListener najdete v tématu [WebListener](xref:fundamentals/servers/weblistener).

![Weblistener komunikuje přímo s interní sítě](weblistener/_static/weblistener-to-internal.png)

---

## <a name="aspnet-core-server-infrastructure"></a>ASP.NET Core serveru infrastruktury

[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) k dispozici v `Startup.Configure` metoda zpřístupňuje [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) vlastnost typu [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel a ovladač HTTP.sys (WebListener v ASP.NET Core 1.x) vystavit pouze jednu funkci jednotlivých, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ale implementace jiný server může vystavit další funkce.

`IServerAddressesFeature` umožňuje zjistit, port, který je vázaný implementaci serveru za běhu.

## <a name="custom-servers"></a>Vlastní servery

Pokud integrované servery nesplňují požadavky na aplikace, mohou být vytvořeny implementace vlastního serveru. [Open Web Interface pro .NET (OWIN) průvodce](xref:fundamentals/owin) ukazuje, jak napsat [Nowin](https://github.com/Bobris/Nowin)– na základě [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementace. Pouze funkce rozhraní, které aplikace používá potřeba implementace, když minimálně [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) a [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) musí být podporována.

## <a name="server-startup"></a>Při spuštění serveru

Při použití [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio pro Mac](https://www.visualstudio.com/vs/mac/), nebo [Visual Studio Code](https://code.visualstudio.com/), server se spustí, když se aplikace spustí na integrované vývojové prostředí ( IDE). V sadě Visual Studio v systému Windows, můžete profily spuštění použít ke spuštění aplikace a server pomocí [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) nebo konzole. V sadě Visual Studio Code aplikace a server se spustí [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), která aktivuje CoreCLR ladicího programu. Pomocí sady Visual Studio pro Mac, aplikace a server se spustí [Mono konfigurace Soft-režim ladění](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).

Při spouštění aplikace z příkazového řádku ve složce projektu [dotnet spustit](/dotnet/core/tools/dotnet-run) spustí aplikaci a serveru (Kestrel a pouze HTTP.sys). Konfigurace je určena `-c|--configuration` možnost, která je nastaven na hodnotu `Debug` (výchozí) nebo `Release`. Pokud spuštění profily se nacházejí v *launchSettings.json* soubor, použijte `--launch-profile <NAME>` možnost nastavit profil spuštění (například `Development` nebo `Production`). Další informace najdete v tématu [dotnet spustit](/dotnet/core/tools/dotnet-run) a [.NET Core distribuční balení](/dotnet/core/build/distribution-packaging) témata.

## <a name="additional-resources"></a>Další zdroje

* [Kestrel](xref:fundamentals/servers/kestrel)
* [Kestrel službou IIS](xref:fundamentals/servers/aspnet-core-module)
* [Hostování v Linuxu na serveru Nginx](xref:host-and-deploy/linux-nginx)
* [Hostování v Linuxu na serveru Apache](xref:host-and-deploy/linux-apache)
* [Ovladač HTTP.sys](xref:fundamentals/servers/httpsys) (pro technologii ASP.NET základní 1.x najdete v tématu [WebListener](xref:fundamentals/servers/weblistener))
