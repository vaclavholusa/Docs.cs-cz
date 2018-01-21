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
# <a name="web-server-implementations-in-aspnet-core"></a>Implementace webového serveru v ASP.NET Core

Podle [tní Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), a [Ross Jan](https://github.com/Tratcher)

Aplikace ASP.NET Core pracuje s implementaci serveru HTTP v procesu. Implementace server naslouchá pro požadavky protokolu HTTP a zobrazí je do aplikace jako sady [žádosti o funkce](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) složený do `HttpContext`.

ASP.NET Core dodává dvě implementace serveru:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

* [Kestrel](kestrel.md) je na základě server HTTP napříč platformami [libuv](https://github.com/libuv/libuv), knihovny a platformy asynchronní vstupně-výstupní operace.

* [Ovladač HTTP.sys](httpsys.md) je na základě protokolu HTTP pouze pro systém Windows server [ovladač Http.Sys jádra](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

* [Kestrel](kestrel.md) je na základě server HTTP napříč platformami [libuv](https://github.com/libuv/libuv), knihovny a platformy asynchronní vstupně-výstupní operace.

* [WebListener](weblistener.md) je na základě protokolu HTTP pouze pro systém Windows server [ovladač Http.Sys jádra](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

---

## <a name="kestrel"></a>Kestrel

Kestrel je webový server, který je zahrnut ve výchozím nastavení v šablonách nový projekt ASP.NET Core. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Kestrel můžete použít samostatně nebo se *reverzní proxy server*, jako jsou například služby IIS, Nginx nebo Apache. Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování.

![Kestrel komunikuje přímo s Internetu bez reverzní proxy server](kestrel/_static/kestrel-to-internet2.png)

![Kestrel nepřímo komunikuje v Internetu přes reverzní proxy server, například služby IIS, Nginx nebo Apache](kestrel/_static/kestrel-to-internet.png)

Buď konfiguraci &mdash; s nebo bez reverzní proxy server &mdash; lze také použít, pokud Kestrel má přístup pouze k interní síti.

Informace o použití Kestrel s reverzní proxy server, najdete v článku [Úvod do Kestrel](kestrel.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Pokud vaše aplikace přijímá požadavky jenom z interní sítě, můžete použít Kestrel samostatně.

![Kestrel komunikuje přímo s interní sítě](kestrel/_static/kestrel-to-internal.png)

Pokud jste vystavit aplikace k Internetu, musí používat službu IIS, Nginx nebo Apache jako *reverzní proxy server*. Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování, jak je znázorněno v následujícím diagramu.

![Kestrel nepřímo komunikuje v Internetu přes reverzní proxy server, například služby IIS, Nginx nebo Apache](kestrel/_static/kestrel-to-internet.png)

Nejdůležitější důvod pro používání reverzní proxy server pro nasazení okraj (vystavený pro přenosy z Internetu) je zabezpečení. Verze 1.x Kestrel nemají úplný doplněk obranu proti útokům. To zahrnuje, ale není omezen na, příslušné vypršení časových limitů, omezení velikosti a omezení počtu souběžných připojení.

Informace o použití Kestrel s reverzní proxy server, najdete v článku [Úvod do Kestrel](kestrel.md).

---

Nelze použít IIS, Nginx nebo Apache bez Kestrel nebo [implementace vlastního serveru](#custom-servers). ASP.NET Core byl navržen ke spuštění ve svém vlastním procesu, takže může chovat konzistentně napříč platformami. Služba IIS, Nginx a Apache stanovují vlastní procesu spouštění a prostředí; používat přímo, ASP.NET Core muset přizpůsobit potřebám každé z nich. Pomocí na webový server implementace například Kestrel nabízí ASP.NET Core kontrolu nad spuštění procesu a prostředí. Proto namísto pokusu přizpůsobit ASP.NET Core služby IIS, Nginx nebo Apache, je právě nastavit tyto webové servery proxy požadavky na Kestrel. Toto uspořádání umožňuje vaší `Program.Main` a `Startup` třídy být v podstatě stejný bez ohledu na to, kde můžete nasadit.

### <a name="iis-with-kestrel"></a>IIS s Kestrel

Při použití služby IIS nebo IIS Express jako reverzní proxy server pro ASP.NET Core aplikace ASP.NET Core spouští samostatný proces z pracovní proces služby IIS. V procesu služby IIS spouští speciální modul IIS ke koordinaci relace reverzní proxy server.  Toto je *ASP.NET Core modulu*. Primární funkce modulu jádra ASP.NET se spustit aplikaci ASP.NET Core, ho restartovat, když ho dojde k chybě a přenosu dat protokolu HTTP k němu. Další informace najdete v tématu [ASP.NET Core modulu](aspnet-core-module.md). 

### <a name="nginx-with-kestrel"></a>Nginx s Kestrel

Informace o tom, jak používat Nginx v systému Linux jako reverzní proxy server pro Kestrel najdete v tématu [hostitele v systému Linux s Nginx](xref:host-and-deploy/linux-nginx).

### <a name="apache-with-kestrel"></a>Apache s Kestrel

Informace o tom, jak používat Apache v systému Linux jako reverzní proxy server pro Kestrel najdete v tématu [hostitele v systému Linux s Apache](xref:host-and-deploy/linux-apache).

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Pokud vaše aplikace ASP.NET Core spustíte v systému Windows, ovladač HTTP.sys je alternativa k Kestrel. Ovladač HTTP.sys můžete použít pro scénáře, kde vystavit aplikace k Internetu a budete potřebovat HTTP.sys funkce, které Kestrel nepodporuje. 

![Ovladač HTTP.sys komunikuje přímo přes Internet](httpsys/_static/httpsys-to-internet.png)

Ovladač HTTP.sys mohou sloužit také pro aplikace, které jsou viditelné pouze na interní síti. 

![Ovladač HTTP.sys komunikuje přímo s interní sítě](httpsys/_static/httpsys-to-internal.png)

Pro scénáře interní síti Kestrel obecně doporučujeme pro nejlepší výkon; ale v některých případech můžete chtít použít funkci, která nabízí jenom ovladače HTTP.sys. Informace o funkcích, ovladač HTTP.sys najdete v tématu [HTTP.sys](httpsys.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Ovladač HTTP.sys jmenuje WebListener v ASP.NET Core 1.x. Pokud spustíte vaše základní technologie ASP.NET je aplikace v systému Windows, WebListener alternativu, která můžete použít pro scénáře, kde chcete vystavit v aplikaci Internet, ale nelze použít IIS.

![Weblistener komunikuje přímo přes Internet](weblistener/_static/weblistener-to-internet.png)

WebListener také jde použít místo Kestrel pro aplikace, které jsou viditelné pouze na interní síti, pokud potřebujete WebListener funkce, které Kestrel nepodporuje. 

![Weblistener komunikuje přímo s interní sítě](weblistener/_static/weblistener-to-internal.png)

Pro scénáře interní síti Kestrel obecně doporučujeme pro nejlepší výkon, ale v některých případech můžete chtít použít funkci, která nabízí pouze WebListener. Informace o funkcích WebListener najdete v tématu [WebListener](weblistener.md).

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a>Poznámky k nástroji ASP.NET Core serveru infrastruktury

[ `IApplicationBuilder` ](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) k dispozici v `Startup` třída `Configure` metoda zpřístupňuje `ServerFeatures` vlastnost typu [ `IFeatureCollection` ](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel a WebListener vystavují jenom jednu funkci [ `IServerAddressesFeature` ](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ale implementace jiný server může vystavit další funkce.

`IServerAddressesFeature`umožňuje zjistit, který port implementaci serveru je vázána na za běhu.

## <a name="custom-servers"></a>Vlastní servery

Pokud integrované servery nevyhovují vašim potřebám, můžete vytvořit vlastní server implementace. [Open Web Interface pro .NET (OWIN) průvodce](../owin.md) ukazuje, jak napsat [Nowin](https://github.com/Bobris/Nowin)– na základě [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementace. Jste volné implementovat právě funkce rozhraní aplikace potřebuje, když minimálně musí podporovat [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) a [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).

## <a name="next-steps"></a>Další kroky

Další informace naleznete v následujících materiálech:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

- [Kestrel](kestrel.md)
- [Kestrel službou IIS](aspnet-core-module.md)
- [Hostování v Linuxu na serveru Nginx](xref:host-and-deploy/linux-nginx)
- [Hostování v Linuxu na serveru Apache](xref:host-and-deploy/linux-apache)
- [HTTP.sys](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

- [Kestrel](kestrel.md)
- [Kestrel službou IIS](aspnet-core-module.md)
- [Hostování v Linuxu na serveru Nginx](xref:host-and-deploy/linux-nginx)
- [Hostování v Linuxu na serveru Apache](xref:host-and-deploy/linux-apache)
- [WebListener](weblistener.md)

---
