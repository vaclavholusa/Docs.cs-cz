---
title: Kestrel webového serveru implementace v ASP.NET Core
author: rick-anderson
description: Další informace o Kestrel napříč platformami webovém serveru pro ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/02/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 62649351271deebcf1ed9d2f8b2258bed3478989
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276653"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a>Kestrel webového serveru implementace v ASP.NET Core

Podle [tní Dykstra](https://github.com/tdykstra), [Jan Ross](https://github.com/Tratcher), a [Stephen Halter](https://twitter.com/halter73)

Kestrel je napříč platformami [webového serveru pro ASP.NET Core](xref:fundamentals/servers/index). Kestrel je webový server, který je zahrnut ve výchozím nastavení v šablony projektů ASP.NET Core.

Kestrel podporuje následující funkce:

* HTTPS
* Neprůhledné upgrade používaným pro povolení [objekty WebSockets](https://github.com/aspnet/websockets)
* Sokety UNIX pro vysoký výkon za Nginx

Kestrel je podporována na všech platformách a verze, které podporuje .NET Core.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Kdy použít Kestrel s reverzní proxy server

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Kestrel můžete použít samostatně nebo se *reverzní proxy server*, jako jsou například služby IIS, Nginx nebo Apache. Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování.

![Kestrel komunikuje přímo s Internetu bez reverzní proxy server](kestrel/_static/kestrel-to-internet2.png)

![Kestrel nepřímo komunikuje v Internetu přes reverzní proxy server, například služby IIS, Nginx nebo Apache](kestrel/_static/kestrel-to-internet.png)

Buď konfiguraci&mdash;s nebo bez reverzní proxy server&mdash;je platný a podporované konfigurace hostování pro technologii ASP.NET Core 2.0 nebo novější.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Pokud aplikace přijímá požadavky jenom z interní sítě, Kestrel můžete použít přímo jako server aplikace.

![Kestrel komunikuje přímo s interní sítě](kestrel/_static/kestrel-to-internal.png)

Pokud vystavit aplikace k Internetu, použít službu IIS, Nginx nebo Apache jako *reverzní proxy server*. Reverzní proxy server přijímá požadavky HTTP z Internetu a předává je Kestrel po některé předběžné zpracování.

![Kestrel nepřímo komunikuje v Internetu přes reverzní proxy server, například služby IIS, Nginx nebo Apache](kestrel/_static/kestrel-to-internet.png)

Reverzní proxy server je vyžadována pro nasazení okraj (vystavený pro přenosy z Internetu) z bezpečnostních důvodů. Verze 1.x Kestrel nemají úplný doplněk obrany před útoky, například odpovídající vypršení časových limitů, omezení velikosti a omezení počtu souběžných připojení.

---

Scénář reverzní proxy server existuje, pokud existuje víc aplikací, které sdílejí stejnou adresu IP a portu spouští na jednom serveru. Kestrel nepodporuje tento scénář, protože Kestrel nepodporuje sdílení stejné IP adresy a portu mezi více procesů. Když Kestrel je nakonfigurován pro naslouchání na portu, Kestrel zpracovává veškeré přenosy dat pro tento port bez ohledu na to žádostí Hlavička hostitele. Reverzní proxy server, můžete sdílet porty má schopnost směrování žádostí Kestrel na jedinečné IP adresy a portu.

I když není požadovaných reverzní proxy server, pomocí reverzní proxy server může být vhodné použít:

* Ho můžete omezit zveřejněné veřejnou oblast aplikací, které ji hostuje.
* Poskytuje další úroveň konfigurace a obrany.
* Může integrovat lépe stávající infrastruktury.
* Usnadňují Vyrovnávání zatížení a konfiguraci šifrování SSL. Pouze reverzní proxy server vyžaduje certifikát SSL a tento server může komunikovat s vašimi aplikací servery v interní síti pomocí prostý protokolu HTTP.

> [!WARNING]
> Pokud nepoužíváte reverzní proxy server s hostitelem filtrování povoleno, [hostitele filtrování](#host-filtering) musí být povolena.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Jak používat Kestrel v aplikacích ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) balíček je součástí [Microsoft.AspNetCore.App metapackage] (xref:fundamentals / metapackage aplikace) (ASP.NET Core 2.1 nebo vyšší).

Šablony projektů ASP.NET Core pomocí Kestrel ve výchozím nastavení. V *Program.cs*, kód zavolá metodu šablony [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), který volá [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) na pozadí.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Nainstalujte [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) balíček NuGet.

Volání [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) rozšiřující metody na [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) v `Main` metoda, zadání žádné [Kestrel možnosti](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) potřeba, jak je znázorněno v následujícím oddílu.

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Možnosti kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

Kestrel webového serveru obsahuje možnosti konfigurace omezení, které jsou užitečné zejména v internetových nasazení. Několik důležitých omezení, které se dají přizpůsobit:

* Maximální počet klientských připojení
* Velikost textu maximální požadavku
* Minimální požadavek textu přenosová rychlost

Nastavit na tyto a další omezení [omezení](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) vlastnost [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) třídy. `Limits` Vlastnost obsahuje instanci [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) třídy.

**Maximální počet klientských připojení**

[MaxConcurrentConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[MaxConcurrentUpgradedConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

Maximální počet souběžných otevřete připojení TCP lze nastavit pro celou aplikaci s následujícím kódem:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

Je samostatný limit pro připojení, které byly upgradované z protokolu HTTP nebo HTTPS na jiný protokol (například pro objekty WebSockets požadavek). Po upgradu připojení není započítané `MaxConcurrentConnections` limit.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

Maximální počet připojení je neomezená (null) ve výchozím nastavení.

**Velikost textu maximální požadavku**

[MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

Výchozí velikost těla maximální požadavku je 30,000,000 bajtů, což je přibližně 28.6 MB.

Doporučeným přístupem k přepsání omezení v aplikaci ASP.NET MVC základní se má používat [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) atribut na metodu akce:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Tady je příklad, který ukazuje, jak nakonfigurovat omezení pro aplikaci na každou žádost:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

Můžete přepsat nastavení konkrétního požadavku v middlewaru.:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

Když zkusíte nakonfigurovat limit na požadavek po spuštění aplikace přečíst požadavek, je vyvolána výjimka. Došlo `IsReadOnly` vlastnost, která určuje, pokud `MaxRequestBodySize` vlastnost je ve stavu jen pro čtení, tzn. je příliš pozdě Konfigurace limitu.

**Minimální požadavek textu přenosová rychlost**

[MinRequestBodyDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[MinResponseDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

Pokud data je přicházejících na určenou míru v bajtech za sekundu, zkontroluje kestrel každou sekundu. Pokud rychlost klesne pod minimální, vypršení časového limitu připojení. Poskytnutá lhůta je množství času, aby Kestrel dává klienta ke zvýšení jeho rychlost odesílání až minimální; rychlost, jakou kontrolována během této doby. Období odkladu pomáhá zabránit, vyřazení připojení, které jsou původně odesílání dat s nízkou rychlostí z důvodu zpomalit TCP-start.

Výchozí minimální rychlost je 240 bajtů za sekundu s období odkladu 5 sekund.

Minimální rychlost platí také pro odpověď. Kód pro nastavení limitu požadavků a omezení odpovědi je stejný kromě nutnosti `RequestBody` nebo `Response` v názvech vlastností a rozhraní.

Tady je příklad, který ukazuje, jak konfigurovat minimální datové sazby v *Program.cs*:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-7)]

Sazby za žádosti můžete nakonfigurovat v middlewaru:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

Informace o dalších možnostech Kestrel a omezení najdete v tématu:

* [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Informace o možnostech Kestrel a omezení najdete v tématu:

* [KestrelServerOptions – třída](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

---

### <a name="endpoint-configuration"></a>Konfigurace koncového bodu

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
Ve výchozím nastavení, ASP.NET Core váže k `http://localhost:5000`. Volání [naslouchání](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) nebo [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) metody na [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) konfigurace předpony adres URL a portů pro Kestrel. `UseUrls`, `--urls` argument příkazového řádku, `urls` klíč konfigurace hostitele a `ASPNETCORE_URLS` také pracovní proměnnou prostředí ale mají omezení později uvedených v této části.

`urls` Klíč konfigurace hostitele musí pocházet z konfigurace hostitele, není konfigurace aplikace. Přidání `urls` klíče a hodnoty na *appSettings.JSON určený* konfigurace hostitele nemá vliv, protože hostitel je zcela inicializován v době konfigurace je pro čtení z konfiguračního souboru. Ale `urls` klíče v *appSettings.JSON určený* lze použít s [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) na tvůrce hostitele, který má konfigurace hostitele:

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```

::: moniker-end
::: moniker range=">= aspnetcore-2.1"

Ve výchozím nastavení ASP.NET Core váže k:

* `http://localhost:5000`
* `https://localhost:5001` (když místní vývojový certifikát je k dispozici)

Vývojový certifikát se vytvoří:

* Když [.NET Core SDK](/dotnet/core/sdk) je nainstalovaná.
* [Dev certifikátů nástroj](xref:aspnetcore-2.1#https) se používá k vytvoření certifikátu.

Některé prohlížeče vyžadují, že udělíte výslovná oprávnění do prohlížeče tak, aby důvěřoval místní vývojový certifikát.

ASP.NET Core 2.1 a novější šablony projektů konfigurace aplikací pro spuštění na HTTPS ve výchozím nastavení a zahrnují [podporují přesměrování protokolu HTTPS a HSTS](xref:security/enforcing-ssl).

Volání [naslouchání](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) nebo [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) metody na [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) konfigurace předpony adres URL a portů pro Kestrel.

`UseUrls`, `--urls` argument příkazového řádku, `urls` klíč konfigurace hostitele a `ASPNETCORE_URLS` také pracovní proměnnou prostředí ale mají omezení později uvedených v této části (výchozí certifikát musí být k dispozici pro koncový bod HTTPS Konfigurace).

ASP.NET Core 2.1 `KestrelServerOptions` konfigurace:

**ConfigureEndpointDefaults (akce&lt;ListenOptions&gt;)**  
Určuje konfiguraci `Action` ke spuštění pro každý zadaný koncový bod. Volání metody `ConfigureEndpointDefaults` vícekrát nahrazuje před `Action`s s poslední `Action` zadaný.

**ConfigureHttpsDefaults (akce&lt;HttpsConnectionAdapterOptions&gt;)**  
Určuje konfiguraci `Action` ke spuštění pro každý koncový bod HTTPS. Volání metody `ConfigureHttpsDefaults` vícekrát nahrazuje před `Action`s s poslední `Action` zadaný.

**Configure(IConfiguration)**  
Vytvoří zavaděč konfigurace pro nastavení Kestrel, který přebírá [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) jako vstup. Konfigurace musí být určené ke konfiguračnímu oddílu pro Kestrel.

**ListenOptions.UseHttps**  
Nakonfigurujte Kestrel pro použití protokolu HTTPS.

`ListenOptions.UseHttps` rozšíření:

* `UseHttps` &ndash; Nakonfigurujte Kestrel pro použití protokolu HTTPS s výchozí certifikát. Vyvolá výjimku, pokud je nakonfigurovaný žádný výchozí certifikát.
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

`ListenOptions.UseHttps` Parametry:

* `filename` je název a cesta k souboru soubor certifikátu, relativně k adresáři, který obsahuje soubory obsahu aplikace.
* `password` je požadováno pro přístup k datům certifikát X.509 heslo.
* `configureOptions` je `Action` nakonfigurovat `HttpsConnectionAdapterOptions`. Vrátí `ListenOptions`.
* `storeName` je úložiště certifikátů pro načtení certifikátu.
* `subject` je název subjektu certifikátu.
* `allowInvalid` Označuje, pokud neplatný certifikáty musí vzít v úvahu například certifikáty podepsané svým držitelem.
* `location` je načíst certifikát z umístění úložiště.
* `serverCertificate` je certifikát X.509.

V produkčním prostředí musí být explicitně nakonfigurovaný protokol HTTPS. Minimálně je třeba zadat výchozí certifikát.

Podporované konfigurace popsána dále:

* Žádná konfigurace
* Nahraďte výchozí certifikát z konfigurace
* Změnit výchozí nastavení v kódu

*Žádná konfigurace*

Kestrel naslouchá na `http://localhost:5000` a `https://localhost:5001` (Pokud je výchozí certifikát je k dispozici).

Určení adres URL pomocí:

* `ASPNETCORE_URLS` Proměnné prostředí.
* `--urls` argument příkazového řádku.
* `urls` Klíč konfigurace hostitele.
* `UseUrls` metody rozšíření.

Další informace najdete v tématu [adresy URL serveru](xref:fundamentals/host/web-host#server-urls) a [konfigurace přepisování](xref:fundamentals/host/web-host#override-configuration).

Hodnota zadaná pomocí těchto přístupů může být jeden nebo více HTTP a HTTPS koncových bodů (HTTPS Pokud je k dispozici certifikát výchozí). Nakonfigurovat hodnotu jako seznam oddělený středníkem (například `"Urls": "http://localhost:8000;http://localhost:8001"`).

*Nahraďte výchozí certifikát z konfigurace*

[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) volání `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` ve výchozím nastavení se načíst konfiguraci Kestrel. Schéma výchozí HTTPS aplikace nastavení konfigurace je k dispozici pro Kestrel. Nakonfigurujte několik koncových bodů, včetně adres URL a certifikátů, které chcete použít, ze souboru na disku nebo z úložiště certifikátů.

V následujícím *appSettings.JSON určený* příklad:

* Nastavit **AllowInvalid** k `true` k povolení použití neplatný certifikátů (například certifikáty podepsané svým držitelem).
* Žádný koncový bod HTTPS, který není určete certifikát (**HttpsDefaultCert** v příkladu, který následuje) spadne zpět na cert definované v části **certifikáty** > **výchozí**  nebo vývojový certifikát.

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

Alternativu k použití **cesta** a **heslo** pro jakýkoliv certifikát je uzel zadejte certifikát pomocí polí úložiště certifikátu. Například **certifikáty** > **výchozí** certifikát lze zadat jako:

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

Schéma poznámky:

* Názvy koncových bodů jsou velká a malá písmena. Například `HTTPS` a `Https` jsou platné.
* `Url` Parametr je vyžadován pro každý koncový bod. Formát pro tento parametr je stejný jako nejvyšší úrovně `Urls` parametr konfigurace s výjimkou, že je omezena na jednu hodnotu.
* Tyto koncové body nahradit názvům definovaným v nejvyšší úrovně `Urls` konfigurace spíše než přidávání na ně. Koncové body definované v kódu prostřednictvím `Listen` jsou kumulativní s koncovými body definované v konfiguračním oddílu.
* `Certificate` Část je nepovinná. Pokud `Certificate` části nezadáte, budou použity výchozí hodnoty definované v předchozích scénáře. Pokud jsou k dispozici žádné výchozí hodnoty, serveru vyvolá výjimku a nepodaří spustit.
* `Certificate` Části podporuje obě **cesta**&ndash;**heslo** a **subjektu**&ndash;**úložiště** certifikáty.
* Tímto způsobem může být definována libovolný počet koncových bodů, tak dlouho, dokud se nezpůsobí, že port je v konfliktu.
* `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` Vrátí `KestrelConfigurationLoader` s `.Endpoint(string name, options => { })` metoda, která umožňuje doplnit nakonfigurovaná nastavení pro koncový bod:

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  Můžete rovněž přímo přistupovat ke `KestrelServerOptions.ConfigurationLoader` zachovat procházení na existující zavaděč, jako je třeba poskytované [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

* Konfigurační oddíl pro každý koncový bod je k dispozici na možnosti v `Endpoint` metoda tak, aby mohou číst vlastní nastavení.
* Více konfigurací může načíst voláním `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` znovu s jiným oddílem. Pouze poslední konfigurace se používá, pokud `Load` je explicitně volána na předchozí instance. Není volání metapackage `Load` tak, aby jeho výchozí konfigurační oddíl lze nahradit.
* `KestrelConfigurationLoader` zrcadlení `Listen` rodiny rozhraní API z `KestrelServerOptions` jako `Endpoint` přetížení, takže kód a konfigurace koncových bodů může být nakonfigurována na stejném místě. Tato přetížení nepoužívat názvy a jenom využívat výchozí nastavení z konfigurace.

*Změnit výchozí nastavení v kódu*

`ConfigureEndpointDefaults` a `ConfigureHttpsDefaults` lze změnit výchozí nastavení pro `ListenOptions` a `HttpsConnectionAdapterOptions`, včetně přepsání výchozí certifikát uvedený v předchozím scénáři. `ConfigureEndpointDefaults` a `ConfigureHttpsDefaults` by měla být volána, než jsou nakonfigurovány žádné koncové body.

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

*Podpora kestrel SNI*

[Indikace názvu serveru (SNI)](https://tools.ietf.org/html/rfc6066#section-3) lze použít k hostování více domén na stejnou IP adresu a port. Pro SNI funkce klient odešle název hostitele pro zabezpečené relace na server během metody handshake TLS, aby server můžete zadejte správný certifikát. Klient použije certifikát zařízená šifrovanou komunikaci se serverem během zabezpečené relace, který následuje TLS handshake.

Kestrel podporuje SNI prostřednictvím `ServerCertificateSelector` zpětného volání. Zpětné volání je vyvolána jednou na připojení k povolit aplikaci zkontrolujte název hostitele a vyberte příslušný certifikát.

Podpora SNI vyžaduje:

* Spuštěná na cílové rozhraní `netcoreapp2.1`. Na `netcoreapp2.0` a `net461`, zpětné volání je voláno, ale `name` je vždy `null`. `name` Je také `null` Pokud klient neposkytuje hostitele parametr name ve TLS handshake.
* Všechny weby na stejnou instanci Kestrel spustit. Kestrel nepodporuje sdílení adresu IP a portu ve více instancích bez reverzní proxy server.

```csharp
WebHost.CreateDefaultBuilder()
    .UseKestrel((context, options) =>
    {
        options.ListenAnyIP(5005, listenOptions =>
        {
            listenOptions.UseHttps(httpsOptions =>
            {
                var localhostCert = CertificateLoader.LoadFromStoreCert(
                    "localhost", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var exampleCert = CertificateLoader.LoadFromStoreCert(
                    "example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var subExampleCert = CertificateLoader.LoadFromStoreCert(
                    "sub.example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var certs = new Dictionary(StringComparer.OrdinalIgnoreCase);
                certs["localhost"] = localhostCert;
                certs["example.com"] = exampleCert;
                certs["sub.example.com"] = subExampleCert;

                httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                {
                    if (name != null && certs.TryGetValue(name, out var cert))
                    {
                        return cert;
                    }

                    return exampleCert;
                };
            });
        });
    });
```

::: moniker-end

**Vytvoření vazby na soket TCP**

[Naslouchání](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) metoda váže na soket TCP a umožňuje lambda možností konfigurace certifikátu protokolu SSL:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

Tento příklad konfiguruje SSL pro koncový bod s [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions). Konfigurovat další nastavení Kestrel pro specifické koncové body pomocí stejné rozhraní API.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

**Vytvoření vazby na soket systému Unix**

Naslouchání soketu Unix s [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) pro zlepšení výkonu s Nginx, jak je uvedeno v následujícím příkladu:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

**Port 0**

Když číslo portu `0` není zadaný, Kestrel dynamicky váže k dostupný port. Následující příklad ukazuje, jak určit port, který Kestrel ve skutečnosti vázaný za běhu:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

Když se aplikace spustí, určuje, výstup okna konzoly dynamický port, kde jsou dostupné aplikace:

```console
Listening on the following addresses: http://127.0.0.1:48508
```

**UseUrls, argument příkazového řádku – adresy URL, klíč konfigurace hostitele adresy URL a proměnných omezení ASPNETCORE_URLS prostředí**

Nakonfigurujte koncové body pomocí následujících postupů:

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* `--urls` Argument příkazového řádku
* `urls` Klíč konfigurace hostitele
* `ASPNETCORE_URLS` Proměnné prostředí

Tyto metody jsou užitečné pro vytváření kódu spolupráci se servery než Kestrel. Ale mějte na paměti tato omezení:

* SSL nelze použít s těmito dvěma způsoby, pokud výchozí certifikát je součástí konfigurace koncového bodu protokolu HTTPS (například pomocí `KestrelServerOptions` konfigurace a konfigurační soubory, jak je znázorněno v tomto tématu výše).
* Při jak `Listen` a `UseUrls` přístupy používají současně, `Listen` přepsat koncové body `UseUrls` koncové body.

**Konfigurace koncového bodu služby IIS**

Při použití IIS, vazby adresu URL pro službu IIS přepsat vazby jsou nastavené buď `Listen` nebo `UseUrls`. Další informace najdete v tématu [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) tématu.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Ve výchozím nastavení, ASP.NET Core váže k `http://localhost:5000`. Konfigurace předpony adres URL a portů pro používání Kestrel:

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) rozšiřující metoda
* `--urls` Argument příkazového řádku
* `urls` Klíč konfigurace hostitele
* ASP.NET Core konfigurace systému, včetně `ASPNETCORE_URLS` proměnné prostředí

Další informace o těchto metodách v tématu [hostitelský](xref:fundamentals/host/index).

**Konfigurace koncového bodu služby IIS**

Při použití IIS, vazby adresu URL pro službu IIS přepsat vazby, která nastavuje `UseUrls`. Další informace najdete v tématu [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) tématu.

---

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a>Konfigurace přenosu

Ve verzi ASP.NET Core 2.1 Kestrel na výchozí přenos je už podle Libuv ale na spravované sokety. Jedná se o změnu ukončování řádků pro upgrade na 2.1, které volat aplikace ASP.NET 2.0 základní [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) a závisí na některý z následujících balíčků:

* [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (přímý odkaz na balíček)
* [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

Pro technologii ASP.NET Core 2.1 nebo novější projekty využívající [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) a vyžadují použití Libuv:

* Přidat závislost [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) balíčku do souboru projektu aplikace:

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="2.1.0" />
    ```

* Volání [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

::: moniker-end

### <a name="url-prefixes"></a>Předpony adres URL

Při použití `UseUrls`, `--urls` argument příkazového řádku, `urls` klíč konfigurace hostitele, nebo `ASPNETCORE_URLS` proměnné prostředí, předpony adres URL může být v některém z následujících formátů.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

Platné jsou pouze předpony adres URL protokolu HTTP. Kestrel nepodporuje SSL při konfiguraci adresy URL vazby pomocí `UseUrls`.

* Adresu IPv4 s číslo portu

  ```
  http://65.55.39.10:80/
  ```

  `0.0.0.0` je zvláštní případ, která se sváže s všechny adresy IPv4.

* Adresa IPv6 se číslo portu

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  `[::]` je ekvivalentem IPv6 IPv4 `0.0.0.0`.

* Název hostitele s číslem portu

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Názvy hostitelů `*`, a `+`, nejsou speciální. Nic nebyl rozpoznán jako platná IP adresa nebo `localhost` váže pro všechny IP adresy IPv6 a IPv4. Chcete-li vytvořit vazbu různé názvy hostitelů na jiné aplikace ASP.NET Core na stejný port, použijte [HTTP.sys](xref:fundamentals/servers/httpsys) nebo reverzní proxy server, jako jsou služby IIS, Nginx nebo Apache.

  > [!WARNING]
  > Pokud nepoužíváte reverzní proxy server s hostitelem filtrování povoleno, povolit [hostitele filtrování](#host-filtering).

* Hostitele `localhost` název s číslo nebo zpětné smyčky IP port s číslem portu

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Když `localhost` není zadaný, Kestrel se pokusí vytvořit vazbu na rozhraní zpětné smyčky protokolu IPv4 a IPv6. Pokud požadovaný port je používán jinou službou buď rozhraní zpětné smyčky, Kestrel se nepodaří spustit. Pokud buď rozhraní zpětné smyčky je k dispozici z jiného důvodu (Většina běžně, protože protokol IPv6 není podporován), Kestrel protokoly upozornění.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Adresu IPv4 s číslo portu

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  `0.0.0.0` je zvláštní případ, která se sváže s všechny adresy IPv4.

* Adresa IPv6 se číslo portu

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  `[::]` je ekvivalentem IPv6 IPv4 `0.0.0.0`.

* Název hostitele s číslem portu

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Názvy hostitelů `*`, a `+` nejsou speciální. Všechno, co není rozpoznaný IP adresu nebo `localhost` váže pro všechny IP adresy IPv6 a IPv4. Chcete-li vytvořit vazbu různé názvy hostitelů na jiné aplikace ASP.NET Core na stejný port, použijte [WebListener](xref:fundamentals/servers/weblistener) nebo reverzní proxy server, jako jsou služby IIS, Nginx nebo Apache.

* Hostitele `localhost` název s číslo nebo zpětné smyčky IP port s číslem portu

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

Pokud je číslo portu `0` není zadaný, Kestrel dynamicky váže k dostupný port. Vytvoření vazby na portu `0` povolené pro všechny název hostitele nebo IP adresu s výjimkou pro `localhost`.

Když se aplikace spustí, určuje, výstup okna konzoly dynamický port, kde jsou dostupné aplikace:

```console
Now listening on: http://127.0.0.1:48508
```

**Předpony adres URL pro protokol SSL**

Pokud volání `UseHttps` metoda rozšíření, nezapomeňte zahrnout předpony adres URL s `https:`:

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

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

---

## <a name="host-filtering"></a>Filtrování hostitele

I když Kestrel podporuje konfigurace, například podle předpony `http://example.com:5000`, Kestrel z velké části ignoruje název hostitele. Hostitele `localhost` je zvláštní případ použité pro vazbu na adresy zpětné smyčky. Žádný hostitel, jiné než explicitní IP adresu se váže k všechny veřejné IP adresy. Žádná z těchto informací se používá k ověření požadavku `Host` hlavičky.

::: moniker range="< aspnetcore-2.0"

Jako alternativní řešení hostování za reverzní proxy server s filtrování hlavičky hostitele. Toto je jediný podporovaný scénář pro Kestrel v ASP.NET Core 1.x.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Jako alternativní řešení, použít pro filtrování požadavků podle middlewaru `Host` hlavičky:

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

Zaregistrovat předchozím `HostFilteringMiddleware` v `Startup.Configure`. Všimněte si, že [řazení middleware registrace](xref:fundamentals/middleware/index#ordering) je důležité. K registraci by mělo dojít okamžitě po registraci diagnostiky Middleware (například `app.UseExceptionHandler`).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

Middleware očekává `AllowedHosts` klíče v *appSettings.JSON určený*/*appsettings.\< EnvironmentName > .json*. Hodnota je oddělený středníkem seznam názvů hostitelů bez čísla portů:

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Jako alternativní řešení použijte filtrování Middleware hostitele. Poskytuje Middleware filtrování hostitele [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) balíček, který je součástí [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 nebo vyšší). Middleware přidává [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), který volá [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

Ve výchozím nastavení vypnutá Middleware filtrování hostitele. Chcete-li povolit middleware, definovat `AllowedHosts` klíče v *appSettings.JSON určený*/*appsettings.\< EnvironmentName > .json*. Hodnota je oddělený středníkem seznam názvů hostitelů bez čísla portů:

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

*appSettings.JSON určený*:

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> [Předané hlavičky Middleware](xref:host-and-deploy/proxy-load-balancer) má také [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) možnost. Přesměrovaná Middleware hlavičky a hostitele filtrování Middleware mají podobné funkce pro různé scénáře. Nastavení `AllowedHosts` se předají Middleware hlavičky je vhodné, pokud Hlavička hostitele není zachován při předávání požadavků s reverzní proxy server nebo službu Vyrovnávání zatížení. Nastavení `AllowedHosts` s hostiteli filtrování middlewaru je vhodné při Kestrel slouží jako hraniční server nebo pokud je hlavička hostitele přímo předávat.
>
> Další informace o předávaných Middleware hlavičky, najdete v části [konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).

::: moniker-end

## <a name="additional-resources"></a>Další zdroje

* [Vynucení protokolu HTTPS](xref:security/enforcing-ssl)
* [Kestrel zdrojového kódu](https://github.com/aspnet/KestrelHttpServer)
* [RFC 7230: Syntaxe a směrování zpráv (část 5.4: hostitele)](https://tools.ietf.org/html/rfc7230#section-5.4)
* [Konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer)
