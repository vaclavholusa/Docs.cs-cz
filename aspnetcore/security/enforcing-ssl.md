---
title: Vynutit HTTPS v ASP.NET Core
author: rick-anderson
description: Ukazuje, jak chcete vyžadovat protokol HTTPS/TLS v ASP.NET Core webové aplikace.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 24ab83192ded381b46fab337a986f51fb22b2227
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729493"
---
# <a name="enforce-https-in-aspnet-core"></a>Vynutit HTTPS v ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Tento dokument ukazuje, jak:

* Vyžadovat protokol HTTPS pro všechny požadavky.
* Přesměrujte všechny požadavky HTTP do HTTPS.

> [!WARNING]
> Proveďte **není** použít [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) na webovým rozhraním API, která zobrazit citlivé informace. `RequireHttpsAttribute` stavové kódy HTTP se používá k přesměrování prohlížeče z HTTP do HTTPS. Rozhraní API klienti nemusí pochopit a orientují přesměrování z HTTP do HTTPS. Tito klienti mohou odesílat informace přes protokol HTTP. Webová rozhraní API se musí buď:
>
> * Nelze naslouchat na protokolu HTTP.
> * Ukončení připojení se stavovým kódem 400 (Chybný požadavek) a není požadavek vyřídit.

<a name="require"></a>
## <a name="require-https"></a>Vyžadovat protokol HTTPS

::: moniker range=">= aspnetcore-2.1"

Doporučujeme, abyste všechny webové aplikace ASP.NET Core volat Middleware přesměrování protokolu HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) přesměrovat všechny požadavky HTTP do HTTPS.

Následující kód volání `UseHttpsRedirection` v `Startup` třídy:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Následující kód volání [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) konfigurace middlewaru možností:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Předchozí zvýrazněný kód:

* Nastaví [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode).
* Nastaví HTTPS port 5001.

Automaticky nastavit port následujících mechanismů:

* Middleware může zjišťovat porty prostřednictvím [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) při za následujících podmínek:
  - Kestrel nebo ovladač HTTP.sys používá přímo s koncových bodů HTTPS (platí také pro spuštění aplikace pomocí ladicího programu Visual Studio Code).
  - Pouze **jeden port HTTPS** používá aplikace.
* Visual Studio se používá:
  - Služba IIS Express má povolen protokol HTTPS.
  - *launchSettings.json* nastaví `sslPort` pro službu IIS Express.

> [!NOTE]
> Pokud aplikace běží a za reverzní proxy server (například služby IIS, služba IIS Express), `IServerAddressesFeature` není k dispozici. Port musí být ručně nakonfigurované. Pokud není nastavený port, požadavky nejsou přesměrovat.

Port můžete nakonfigurovat pomocí nastavení:

* `ASPNETCORE_HTTPS_PORT` Proměnné prostředí.
* `http_port` Klíč konfigurace hostitele (například prostřednictvím *hostsettings.json* nebo argument příkazového řádku).
* [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport). Najdete v předchozím příkladu, který ukazuje, jak nastavit port na 5001.

> [!NOTE]
> Port se dá nakonfigurovat nepřímo nastavování adresy URL se `ASPNETCORE_URLS` proměnné prostředí. Proměnné prostředí nakonfiguruje server a pak middleware nepřímo zjišťuje port HTTPS přes `IServerAddressesFeature`.

Pokud je nastaven žádný port:

* Požadavky nejsou přesměrovat.
* Middleware zaznamená upozornění.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se používá k vyžadují protokol HTTPS. `[RequireHttpsAttribute]` můžete uspořádání řadiče nebo metod, nebo mohou být použity globálně. Použít atribut globálně, přidejte následující kód do `ConfigureServices` v `Startup`:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Předchozí zvýrazněný vyžaduje všechny požadavky používat `HTTPS`; proto požadavky HTTP jsou ignorovány. Následující zvýrazněný kód přesměruje všech požadavků HTTP do HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Další informace najdete v tématu [URL přepisování Middleware](xref:fundamentals/url-rewriting).

Globálně vyžadujících protokol HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) je nejlepším postupem zabezpečení. Použití `[RequireHttps]` atribut na všechny řadiče nebo Razor stránky se nepovažuje za bezpečnou jako jako globální vyžadujících protokol HTTPS. Nebudete moct zaručit `[RequireHttps]` atribut se používá, když se přidají nové řadiče a stránky Razor.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>Protokol zabezpečení striktní přenos HTTP (HSTS)

Za [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [striktní přenos HTTP zabezpečení (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) je určený parametrem webové aplikace prostřednictvím hlavičky odpovědi speciální vylepšení zabezpečení opt-in. Jakmile podporovaného prohlížeče obdrží tuto hlavičku prohlížeč tohoto zabrání veškerou komunikaci odesílání prostřednictvím protokolu HTTP k zadané doméně a místo toho odešle veškerou komunikaci přes protokol HTTPS. Rovněž zamezí klikněte na protokol HTTPS přes výzvy v prohlížečích.

Implementuje HSTS s ASP.NET Core 2.1 nebo vyšší `UseHsts` metoda rozšíření. Následující kód volání `UseHsts` Pokud aplikace není v [režimu pro vývoj](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` není doporučujeme při vývoji protože záhlaví HSTS je vysoce cachable pomocí prohlížeče. Ve výchozím nastavení vyloučí UseHsts adresu místní smyčky.

Následující kód:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Nastaví parametr přednačtení záhlaví zabezpečení přenosu Strict. Předběžného načítání není součástí [specifikaci RFC HSTS](https://tools.ietf.org/html/rfc6797), ale podporuje webových prohlížečů přednačtení HSTS weby na čerstvou instalaci. V tématu [ https://hstspreload.org/ ](https://hstspreload.org/) Další informace.
* Umožňuje [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), které se vztahují zásady HSTS na hostiteli subdomény. 
* Explicitně nastaví parametr maximální stáří zabezpečení přenosu Strict hlavičky k na 60 dnů. Pokud není nastavena, výchozí hodnota je 30 dní. Najdete v článku [maximální stáří direktiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) Další informace.
* Přidá `example.com` do seznamu hostitelů, které chcete vyloučit.

`UseHsts` vyloučí následující zpětné smyčky hostitele:

* `localhost` : IPv4 adresu zpětné smyčky.
* `127.0.0.1` : IPv4 adresu zpětné smyčky.
* `[::1]` : IPv6 adresu zpětné smyčky.

Předchozí příklad ukazuje, jak přidat další hostitele.
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Výslovný nesouhlas s HTTPS na vytvoření projektu

Povolit šablony ASP.NET Core 2.1 nebo novější webových aplikací (ze sady Visual Studio nebo pomocí příkazového řádku dotnet) [HTTPS přesměrování](#require) a [HSTS](#hsts). Pro nasazení, které nevyžadují protokol HTTPS které můžete výslovný nesouhlas s protokolu HTTPS. Například není potřeba některé služby back-end, kde HTTPS je zpracovanou externě na okraji a pomocí protokolu HTTPS v každém uzlu.

Vyjádřit výslovný nesouhlas protokolu HTTPS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Zrušte zaškrtnutí políčka **konfigurace pro protokol HTTPS** zaškrtávací políčko.

![Entity diagram](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli) 

Použití `--no-https` možnost. Příklad

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a>Postup instalace certifikátu vývojáře pro Docker

V tématu [potíže Githubu](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end
