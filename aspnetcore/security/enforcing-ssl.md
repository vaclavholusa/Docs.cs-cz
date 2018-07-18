---
title: Vynucení protokolu HTTPS v ASP.NET Core
author: rick-anderson
description: Ukazuje, jak chcete vyžadovat protokol HTTPS/TLS v ASP.NET Core webové aplikace.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 331c17de33b5c13221385ffb4282bc16bde32289
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095714"
---
# <a name="enforce-https-in-aspnet-core"></a>Vynucení protokolu HTTPS v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Tento dokument ukazuje, jak:

* Vyžadovat protokol HTTPS pro všechny požadavky.
* Přesměrujte všechny požadavky HTTP na HTTPS.

> [!WARNING]
> Proveďte **není** použít [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) na webová rozhraní API, která zobrazit citlivé informace. `RequireHttpsAttribute` stavové kódy HTTP se používá pro přesměrování prohlížeče z HTTP na HTTPS. Klienti rozhraní API nemusí pochopit a dodržují přesměrování z HTTP na HTTPS. Tito klienti mohou odesílat informace přes protokol HTTP. Webová rozhraní API by buď:
>
> * Nelze naslouchat na HTTP.
> * Ukončete připojení s stavový kód 400 (Chybný požadavek) a není obsluhovat žádosti.

<a name="require"></a>
## <a name="require-https"></a>Vyžadovat protokol HTTPS

::: moniker range=">= aspnetcore-2.1"

Doporučujeme vám, že všechny webové aplikace ASP.NET Core volat Middleware přesměrování protokolu HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) přesměrovat všechny požadavky HTTP na HTTPS.

Následující kód volá `UseHttpsRedirection` v `Startup` třídy:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Následující kód volá [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) konfigurace middlewaru možností:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Předchozí zvýrazněný kód:

* Nastaví [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) k `Status307TemporaryRedirect`, což je výchozí hodnota. Produkční aplikace by měly volat [UseHsts](#hsts).
* Nastaví HTTPS port 5001. Výchozí hodnota je 443.

Automaticky nastavit port následujících mechanismů:

* Middleware najdou porty prostřednictvím [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) když platí tyto podmínky:
  - Kestrel nebo ovladač HTTP.sys se používá přímo s koncovým bodům protokol HTTPS (platí také pro spuštění aplikace pomocí ladicího programu nástroje Visual Studio Code).
  - Pouze **jeden port HTTPS** používají aplikaci.
* Visual Studio se používá:
  - Služba IIS Express má povolený protokol HTTPS.
  - *launchSettings.json* nastaví `sslPort` pro službu IIS Express.

> [!NOTE]
> Při spuštění aplikace za reverzní proxy server (například služby IIS, IIS Express), `IServerAddressesFeature` není k dispozici. Port musí být ručně nakonfigurované. Pokud port není nastavena, přesměrování požadavků.

Port, který se dá nakonfigurovat pomocí nastavení:

* `ASPNETCORE_HTTPS_PORT` proměnné prostředí.
* `http_port` Konfigurační klíč hostitele (například prostřednictvím *hostsettings.json* nebo argument příkazového řádku).
* [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport). Prohlédněte si předchozí příklad ukazuje, jak nastavit port 5001.

> [!NOTE]
> Port, který se dá nakonfigurovat nepřímo pomocí nastavení adresy URL s `ASPNETCORE_URLS` proměnné prostředí. Proměnná prostředí nakonfiguruje server a pak middleware nepřímo zjistí port HTTPS přes `IServerAddressesFeature`.

Pokud je nastaven žádný port:

* Přesměrování požadavků.
* Middleware zaprotokoluje upozornění.

> [!NOTE]
> Alternativou k používání HTTPS přesměrování Middleware (`UseHttpsRedirection`), je použít Middleware pro přepis adres URL (`AddRedirectToHttps`). `AddRedirectToHttps` Můžete také nastavit stavový kód a portu při spuštění přesměrování. Další informace najdete v tématu [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting).
>
> Při přesměrování na HTTPS bez nutnosti další přesměrování pravidla, doporučujeme používat Middleware přesměrování protokolu HTTPS (`UseHttpsRedirection`) popsané v tomto tématu.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se používá tak, aby vyžadovala protokol HTTPS. `[RequireHttpsAttribute]` můžete uspořádání, řadiče nebo metod, nebo je možné uplatňovat globálně. Použít atribut globálně, přidejte následující kód, který `ConfigureServices` v `Startup`:

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Předchozí zvýrazněný kód technologie používat všechny požadavky `HTTPS`, proto požadavky HTTP jsou ignorovány. Následující zvýrazněný kód přesměruje všechny požadavky HTTP na HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Další informace najdete v tématu [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting). Middleware také umožňuje, že aplikace nastaví stavový kód nebo kód stavu a port, který při spuštění přesměrování.

Globálně vyžadování protokolu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) je z bezpečnostních důvodů. Použití `[RequireHttps]` atribut na všechny řadiče/Razor Pages není považován za stejně bezpečné jako globálně vyžadování protokolu HTTPS. Nebudete moct zaručit, `[RequireHttps]` atributu se použije při přidání nových řadičů a stránky Razor.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>Protokol zabezpečení striktní přenos HTTP (HSTS)

Za [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [striktní přenos HTTP zabezpečení (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) je vylepšení zabezpečení přihlášení, která je zadána webovou aplikaci pomocí hlavičky speciální odpovědi. Jakmile podporovaný prohlížeč obdrží toto záhlaví prohlížeče zabrání jakékoli komunikační nezabránil odesílání prostřednictvím protokolu HTTP k zadané doméně a místo toho pošle veškerou komunikaci přes protokol HTTPS. Zabrání také klikněte na protokol HTTPS pomocí pokynů v prohlížečích.

Implementuje HSTS s ASP.NET Core 2.1 nebo vyšší `UseHsts` – metoda rozšíření. Následující kód volá `UseHsts` když aplikace není v [vývojový režim](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` není doporučeno při vývoji protože záhlaví HSTS je dokonalé prohlížeče. Ve výchozím nastavení `UseHsts` nezahrnuje adresu místní zpětné smyčky.

Následující kód:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Nastaví parametr přednačtení záhlaví zabezpečení přenosu Strict. Předběžné načtení není součástí [specifikaci RFC HSTS](https://tools.ietf.org/html/rfc6797), ale podporuje webových prohlížečů přednačtení HSTS weby na čerstvou instalaci. Zobrazit [ https://hstspreload.org/ ](https://hstspreload.org/) Další informace.
* Umožňuje [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), které se vztahují zásady HSTS na hostiteli subdomény. 
* Explicitně nastaví parametr max-age záhlaví zabezpečení přenosu Strict na 60 dnů. Pokud není nastavena výchozí hodnota je 30 dní. Najdete v článku [max-age směrnice](https://tools.ietf.org/html/rfc6797#section-6.1.1) Další informace.
* Přidá `example.com` do seznamu hostitelů mají vyloučit.

`UseHsts` vyloučí následující zpětné smyčky hostitele:

* `localhost` : IPv4 adresu zpětné smyčky.
* `127.0.0.1` : IPv4 adresu zpětné smyčky.
* `[::1]` : IPv6 adresu zpětné smyčky.

Předchozí příklad ukazuje, jak přidat další hostitele.
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Výslovný nesouhlas s HTTPS při vytvoření projektu

Šablony ASP.NET Core 2.1 nebo novější webových aplikací (ze sady Visual Studio nebo příkazového řádku dotnet) povolit [HTTPS přesměrování](#require) a [HSTS](#hsts). Pro nasazení, které nevyžadují protokol HTTPS je můžete odhlásit protokolu HTTPS. Například některé back-endových služeb, kde HTTPS se zpracovává externě na hraničních zařízeních, pomocí protokolu HTTPS v každém uzlu, není nutná.

Odhlásit protokolu HTTPS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Zrušte zaškrtnutí políčka **konfigurace pro protokol HTTPS** zaškrtávací políčko.

![Entity diagram](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli) 

Použití `--no-https` možnost. Příklad

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a>Jak nastavit certifikát pro vývojáře pro Docker

Zobrazit [tento problém Githubu](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end
