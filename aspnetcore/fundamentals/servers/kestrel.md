---
title: "Kestrel webového serveru implementace v ASP.NET Core"
author: tdykstra
description: "Představuje Kestrel, a platformy webového serveru pro ASP.NET Core podle libuv."
keywords: "ASP.NET Core, Kestrel, libuv, předpony adres url"
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 560bd66f-7dd0-4e68-b5fb-f31477e4b785
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 451c36fc9095b6e076e5287c992b6163205c523b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>Úvod k implementaci serveru webové Kestrel v ASP.NET Core

Podle [tní Dykstra](https://github.com/tdykstra), [Jan Ross](https://github.com/Tratcher), a [Stephen Halter](https://twitter.com/halter73)

Kestrel je napříč platformami [webového serveru pro ASP.NET Core](index.md) na základě [libuv](https://github.com/libuv/libuv), knihovny a platformy asynchronní vstupně-výstupní operace. Kestrel je webový server, který je zahrnut ve výchozím nastavení v šablony projektů ASP.NET Core. 

Kestrel podporuje následující funkce:

  * PROTOKOL HTTPS
  * Neprůhledné upgrade používaným pro povolení [objekty WebSockets](https://github.com/aspnet/websockets)
  * Sokety UNIX pro vysoký výkon za Nginx 

Kestrel je podporována na všech platformách a verze, které podporuje .NET Core.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[Zobrazit nebo stáhnout ukázkový kód pro 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([stažení](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

[Zobrazit nebo stáhnout ukázkový kód pro 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([stažení](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Kdy použít Kestrel s reverzní proxy server

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Kestrel můžete použít samostatně nebo se *reverzní proxy server*, jako jsou například služby IIS, Nginx nebo Apache. Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování.

![Kestrel komunikuje přímo s Internetu bez reverzní proxy server](kestrel/_static/kestrel-to-internet2.png)

![Kestrel nepřímo komunikuje v Internetu přes reverzní proxy server, například služby IIS, Nginx nebo Apache](kestrel/_static/kestrel-to-internet.png)

Buď konfiguraci &mdash; s nebo bez reverzní proxy server &mdash; lze také použít, pokud Kestrel má přístup pouze k interní síti.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Pokud vaše aplikace přijímá požadavky jenom z interní sítě, můžete použít Kestrel samostatně.

![Kestrel komunikuje přímo s interní sítě](kestrel/_static/kestrel-to-internal.png)

Pokud jste vystavit aplikace k Internetu, musí používat službu IIS, Nginx nebo Apache jako *reverzní proxy server*. Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování.

![Kestrel nepřímo komunikuje v Internetu přes reverzní proxy server, například služby IIS, Nginx nebo Apache](kestrel/_static/kestrel-to-internet.png)

Reverzní proxy server je vyžadována pro nasazení okraj (vystavený pro přenosy z Internetu) z bezpečnostních důvodů. Verze 1.x Kestrel nemají úplný doplněk obranu proti útokům. To zahrnuje, avšak není omezeno na příslušné vypršení časových limitů, omezení velikosti a omezení počtu souběžných připojení.

---

Scénáře, který vyžaduje reverzní proxy server je, když máte více aplikací, které sdílejí stejnou adresu IP a portu spouští na jednom serveru. To není možné s Kestrel přímo Kestrel, který nepodporuje sdílení stejné IP adresy a portu mezi více procesy. Když konfigurujete Kestrel naslouchat na portu, zpracovává všechny přenosy pro tento port bez ohledu na hlavičku hostitele. Reverzní proxy server, můžete sdílet porty musí předat pak Kestrel na jedinečné IP adresy a portu.

I když není požadovaných reverzní proxy server, pomocí jednoho může být vhodný z jiných důvodů:

* Ho můžete omezit vystavených útoku.
* Poskytuje další úroveň volitelné konfigurace a obrany.
* Může integrovat lépe stávající infrastruktury.
* Usnadňují Vyrovnávání zatížení a nastavení protokolu SSL. Pouze reverzní proxy server vyžaduje certifikát SSL a tento server může komunikovat s vašimi aplikací servery v interní síti pomocí prostý protokolu HTTP.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Jak používat Kestrel v aplikacích ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) je součástí balíčku [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

Šablony projektů ASP.NET Core pomocí Kestrel ve výchozím nastavení. V *Program.cs*, kód zavolá metodu šablony `CreateDefaultBuilder`, který volá [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) na pozadí.

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

Pokud potřebujete nakonfigurovat možnosti Kestrel, zavolejte `UseKestrel` v *Program.cs* jak je znázorněno v následujícím příkladu:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Nainstalujte [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) balíček NuGet.

Volání [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) rozšiřující metody na `WebHostBuilder` v vaše `Main` metoda, zadání žádné [Kestrel možnosti](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) , které potřebujete, jak je znázorněno v následujícím oddílu.

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Možnosti kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Kestrel webového serveru obsahuje možnosti konfigurace omezení, které jsou užitečné zejména v internetových nasazení. Zde naleznete některá omezení, které můžete zadat:

- Maximální počet klientských připojení
- Velikost textu maximální požadavku
- Minimální požadavek textu přenosová rychlost

Nastavení těchto omezení a ostatní uživatele v `Limits` vlastnost [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) třídy. `Limits` Vlastnost obsahuje instanci [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) třídy. 

**Maximální počet klientských připojení**

Maximální počet souběžných otevřete připojení TCP lze nastavit pro celou aplikaci s následujícím kódem:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

Je samostatný limit pro připojení, které byly upgradované z protokolu HTTP nebo HTTPS na jiný protokol (například pro objekty WebSockets požadavek). Po upgradu připojení není započítané `MaxConcurrentConnections` limit. 

Maximální počet připojení je neomezená (null) ve výchozím nastavení.

**Velikost textu maximální požadavku**

Výchozí velikost těla maximální požadavku je 30,000,000 bajtů, což je přibližně 28.6 MB. 

Doporučený způsob přepsat omezení v aplikaci ASP.NET MVC základní se má používat [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atribut na metodu akce:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Tady je příklad, který ukazuje, jak nakonfigurovat omezení pro celou aplikaci, každou žádost:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

Můžete přepsat nastavení konkrétního požadavku v middlewaru.:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
Pokud se pokusíte nakonfigurovat limit na požadavek po spuštění aplikace čtení požadavku, je vyvolána výjimka. Je `IsReadOnly` vlastnost, která se dozvíte, pokud `MaxRequestBodySize` vlastnost je ve stavu jen pro čtení, tzn. je příliš pozdě Konfigurace limitu.

**Minimální požadavek textu přenosová rychlost**

Kestrel kontroluje každou sekundu v případě pochází data na určenou míru v bajtech za sekundu. Pokud rychlost klesne pod minimální, vypršení časového limitu připojení. Poskytnutá lhůta je množství času, aby Kestrel dává klienta ke zvýšení jeho rychlost odesílání až minimální; rychlost, jakou kontrolována během této doby. Období odkladu pomáhá zabránit, vyřazení připojení, které jsou původně odesílání dat s nízkou rychlostí z důvodu zpomalit TCP-start.

Výchozí minimální rychlost je 240 bajtů za sekundu, období odkladu 5 sekund.

Minimální rychlost platí také pro odpověď. Kód pro nastavení limitu požadavků a omezení odpovědi je stejný kromě nutnosti `RequestBody` nebo `Response` v názvech vlastností a rozhraní. 

Tady je příklad, který ukazuje, jak konfigurovat minimální datové sazby v *Program.cs*:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

Sazby za žádosti můžete nakonfigurovat v middlewaru:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

Informace o dalších možnostech Kestrel najdete v tématu následující třídy:

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Informace o možnostech Kestrel najdete v tématu [KestrelServerOptions třída](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).

---

### <a name="endpoint-configuration"></a>Konfigurace koncového bodu

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Ve výchozím nastavení ASP.NET Core váže k `http://localhost:5000`. Konfigurace předpony adres URL a portů pro Kestrel tak, aby naslouchala na voláním `Listen` nebo `ListenUnixSocket` metody na `KestrelServerOptions`. (`UseUrls`, `urls` argument příkazového řádku a proměnnou prostředí ASPNETCORE_URLS také pracovní ale mají omezení uvedeno [dále v tomto článku](#useurls-limitations).)

**Vytvoření vazby na soket TCP**

`Listen` Metoda váže na soket TCP a lambda možnosti vám umožní nakonfigurovat certifikát SSL:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

Všimněte si, jak tento příklad konfiguruje SSL pro koncový bod konkrétní pomocí [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs). Konfigurovat další nastavení Kestrel pro konkrétní koncové body, můžete použít stejné rozhraní API.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**Vytvoření vazby na soket systému Unix**

Můžete naslouchání pro zlepšení výkonu s Nginx, soketu Unix, jak je uvedeno v následujícím příkladu:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**Port 0**

Pokud zadáte číslo portu 0, Kestrel dynamicky sváže dostupný port. Následující příklad ukazuje, jak určit port, který Kestrel ve skutečnosti vázaný za běhu:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**Omezení UseUrls**

Koncové body můžete nakonfigurovat pomocí volání `UseUrls` metoda nebo pomocí `urls` argument příkazového řádku nebo proměnná prostředí ASPNETCORE_URLS. Tyto metody jsou užitečné, pokud chcete, aby váš kód pro práci se servery než Kestrel. Ale mějte na paměti tato omezení:

* Pomocí těchto metod, nelze používat protokol SSL.
* Pokud používáte obě `Listen` metoda a `UseUrls`, `Listen` přepsat koncové body `UseUrls` koncové body.

**Konfigurace koncového bodu pro službu IIS**

Pokud používáte službu IIS, vazby adresu URL pro službu IIS přepsat žádné vazby, která jste nastavili voláním buď `Listen` nebo `UseUrls`. Další informace najdete v tématu [Úvod k modulu jádra ASP.NET](aspnet-core-module.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Ve výchozím nastavení ASP.NET Core váže k `http://localhost:5000`. Můžete nakonfigurovat předpony adres URL a portů pro Kestrel tak, aby naslouchala na pomocí `UseUrls` metoda rozšíření `urls` argument příkazového řádku nebo konfigurační systém ASP.NET Core. Další informace o těchto metodách v tématu [hostitelský](../../fundamentals/hosting.md). Informace o fungování vazby adresy URL při použití IIS jako reverzní proxy server, najdete v článku [ASP.NET Core modulu](aspnet-core-module.md). 

---

### <a name="url-prefixes"></a>Předpony adres URL

Když zavoláte `UseUrls` nebo použít `urls` argument příkazového řádku nebo proměnná prostředí ASPNETCORE_URLS předpony adres URL může být v některém z následujících formátů. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Pouze předpony adres URL protokolu HTTP jsou platné; Kestrel SSL nepodporuje, když konfigurujete vazby adresu URL pomocí `UseUrls`.

* Adresu IPv4 s číslo portu

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 je zvláštní případ, která se sváže s všechny adresy IPv4.


* Adresa IPv6 se číslo portu

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [:] je ekvivalentem IPv6 IPv4 0.0.0.0.


* Název hostitele s číslem portu

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Názvy hostitelů *, a + nejsou speciální. Všechno, co není rozpoznaný IP adresu nebo "localhost" vytvoří vazbu pro všechny IP adresy IPv6 a IPv4. Pokud potřebujete vytvořit vazbu různé názvy hostitelů na různé aplikace ASP.NET Core na stejném portu, použijte [HTTP.sys](httpsys.md) nebo reverzní proxy server, jako jsou služby IIS, Nginx nebo Apache.

* Název "Localhost" s IP číslo nebo zpětné smyčky port s číslem portu

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Když `localhost` není zadaný, Kestrel se pokusí vytvořit vazbu na rozhraní zpětné smyčky protokolu IPv4 a IPv6. Pokud požadovaný port je používán jinou službou buď rozhraní zpětné smyčky, Kestrel se nepodaří spustit. Pokud buď rozhraní zpětné smyčky je k dispozici z jiného důvodu (Většina běžně, protože protokol IPv6 není podporován), Kestrel protokoly upozornění. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

* Adresu IPv4 s číslo portu

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 je zvláštní případ, která se sváže s všechny adresy IPv4.


* Adresa IPv6 se číslo portu

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [:] je ekvivalentem IPv6 IPv4 0.0.0.0.


* Název hostitele s číslem portu

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Názvy hostitelů \*, a + nejsou speciální. Váže všechno, co není rozpoznaný IP adresu nebo "localhost" pro všechny IP adresy IPv6 a IPv4. Pokud potřebujete vytvořit vazbu různé názvy hostitelů na různé aplikace ASP.NET Core na stejném portu, použijte [WebListener](weblistener.md) nebo reverzní proxy server, jako jsou služby IIS, Nginx nebo Apache.

* Název "Localhost" s IP číslo nebo zpětné smyčky port s číslem portu

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Když `localhost` není zadaný, Kestrel se pokusí vytvořit vazbu na rozhraní zpětné smyčky protokolu IPv4 a IPv6. Pokud požadovaný port je používán jinou službou buď rozhraní zpětné smyčky, Kestrel se nepodaří spustit. Pokud buď rozhraní zpětné smyčky je k dispozici z jiného důvodu (Většina běžně, protože protokol IPv6 není podporován), Kestrel protokoly upozornění. 

* UNIX soketů

  ```
  http://unix:/run/dan-live.sock  
  ```

**Port 0**

Pokud zadáte číslo portu 0, Kestrel dynamicky sváže dostupný port. Vytvoření vazby na portu 0 je povoleno pro všechny název hostitele nebo IP adresu s výjimkou `localhost` název.

Následující příklad ukazuje, jak určit port, který Kestrel ve skutečnosti vázaný za běhu:

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**Předpony adres URL pro protokol SSL**

Nezapomeňte zahrnout předpony adres URL s `https:` při volání `UseHttps` rozšíření metoda, jak je uvedeno níže.

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
> Protokol HTTPS a HTTP nemůže být hostovaná na stejném portu.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>Další kroky

Další informace naleznete v následujících materiálech:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

* [Ukázková aplikace pro 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Kestrel zdrojového kódu](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

* [Ukázková aplikace pro 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Kestrel zdrojového kódu](https://github.com/aspnet/KestrelHttpServer)

---
