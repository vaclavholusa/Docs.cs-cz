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
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="a60a6-103">Vynucení protokolu HTTPS v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a60a6-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="a60a6-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a60a6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a60a6-105">Tento dokument ukazuje, jak:</span><span class="sxs-lookup"><span data-stu-id="a60a6-105">This document shows how to:</span></span>

* <span data-ttu-id="a60a6-106">Vyžadovat protokol HTTPS pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="a60a6-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="a60a6-107">Přesměrujte všechny požadavky HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a60a6-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="a60a6-108">Proveďte **není** použít [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) na webová rozhraní API, která zobrazit citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="a60a6-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="a60a6-109">`RequireHttpsAttribute` stavové kódy HTTP se používá pro přesměrování prohlížeče z HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a60a6-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="a60a6-110">Klienti rozhraní API nemusí pochopit a dodržují přesměrování z HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a60a6-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="a60a6-111">Tito klienti mohou odesílat informace přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="a60a6-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="a60a6-112">Webová rozhraní API by buď:</span><span class="sxs-lookup"><span data-stu-id="a60a6-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="a60a6-113">Nelze naslouchat na HTTP.</span><span class="sxs-lookup"><span data-stu-id="a60a6-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="a60a6-114">Ukončete připojení s stavový kód 400 (Chybný požadavek) a není obsluhovat žádosti.</span><span class="sxs-lookup"><span data-stu-id="a60a6-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="a60a6-115">Vyžadovat protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="a60a6-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a60a6-116">Doporučujeme vám, že všechny webové aplikace ASP.NET Core volat Middleware přesměrování protokolu HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) přesměrovat všechny požadavky HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a60a6-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="a60a6-117">Následující kód volá `UseHttpsRedirection` v `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="a60a6-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="a60a6-118">Následující kód volá [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) konfigurace middlewaru možností:</span><span class="sxs-lookup"><span data-stu-id="a60a6-118">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="a60a6-119">Předchozí zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="a60a6-119">The preceding highlighted code:</span></span>

* <span data-ttu-id="a60a6-120">Nastaví [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) k `Status307TemporaryRedirect`, což je výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="a60a6-120">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="a60a6-121">Produkční aplikace by měly volat [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="a60a6-121">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="a60a6-122">Nastaví HTTPS port 5001.</span><span class="sxs-lookup"><span data-stu-id="a60a6-122">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="a60a6-123">Výchozí hodnota je 443.</span><span class="sxs-lookup"><span data-stu-id="a60a6-123">The default value is 443.</span></span>

<span data-ttu-id="a60a6-124">Automaticky nastavit port následujících mechanismů:</span><span class="sxs-lookup"><span data-stu-id="a60a6-124">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="a60a6-125">Middleware najdou porty prostřednictvím [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) když platí tyto podmínky:</span><span class="sxs-lookup"><span data-stu-id="a60a6-125">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="a60a6-126">Kestrel nebo ovladač HTTP.sys se používá přímo s koncovým bodům protokol HTTPS (platí také pro spuštění aplikace pomocí ladicího programu nástroje Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="a60a6-126">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="a60a6-127">Pouze **jeden port HTTPS** používají aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a60a6-127">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="a60a6-128">Visual Studio se používá:</span><span class="sxs-lookup"><span data-stu-id="a60a6-128">Visual Studio is used:</span></span>
  - <span data-ttu-id="a60a6-129">Služba IIS Express má povolený protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a60a6-129">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="a60a6-130">*launchSettings.json* nastaví `sslPort` pro službu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="a60a6-130">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="a60a6-131">Při spuštění aplikace za reverzní proxy server (například služby IIS, IIS Express), `IServerAddressesFeature` není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a60a6-131">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="a60a6-132">Port musí být ručně nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="a60a6-132">The port must be manually configured.</span></span> <span data-ttu-id="a60a6-133">Pokud port není nastavena, přesměrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="a60a6-133">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="a60a6-134">Port, který se dá nakonfigurovat pomocí nastavení:</span><span class="sxs-lookup"><span data-stu-id="a60a6-134">The port can be configured by setting the:</span></span>

* <span data-ttu-id="a60a6-135">`ASPNETCORE_HTTPS_PORT` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="a60a6-135">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="a60a6-136">`http_port` Konfigurační klíč hostitele (například prostřednictvím *hostsettings.json* nebo argument příkazového řádku).</span><span class="sxs-lookup"><span data-stu-id="a60a6-136">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="a60a6-137">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="a60a6-137">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="a60a6-138">Prohlédněte si předchozí příklad ukazuje, jak nastavit port 5001.</span><span class="sxs-lookup"><span data-stu-id="a60a6-138">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="a60a6-139">Port, který se dá nakonfigurovat nepřímo pomocí nastavení adresy URL s `ASPNETCORE_URLS` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="a60a6-139">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="a60a6-140">Proměnná prostředí nakonfiguruje server a pak middleware nepřímo zjistí port HTTPS přes `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="a60a6-140">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="a60a6-141">Pokud je nastaven žádný port:</span><span class="sxs-lookup"><span data-stu-id="a60a6-141">If no port is set:</span></span>

* <span data-ttu-id="a60a6-142">Přesměrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="a60a6-142">Requests aren't redirected.</span></span>
* <span data-ttu-id="a60a6-143">Middleware zaprotokoluje upozornění.</span><span class="sxs-lookup"><span data-stu-id="a60a6-143">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="a60a6-144">Alternativou k používání HTTPS přesměrování Middleware (`UseHttpsRedirection`), je použít Middleware pro přepis adres URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="a60a6-144">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="a60a6-145">`AddRedirectToHttps` Můžete také nastavit stavový kód a portu při spuštění přesměrování.</span><span class="sxs-lookup"><span data-stu-id="a60a6-145">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="a60a6-146">Další informace najdete v tématu [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="a60a6-146">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="a60a6-147">Při přesměrování na HTTPS bez nutnosti další přesměrování pravidla, doporučujeme používat Middleware přesměrování protokolu HTTPS (`UseHttpsRedirection`) popsané v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="a60a6-147">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="a60a6-148">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se používá tak, aby vyžadovala protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a60a6-148">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="a60a6-149">`[RequireHttpsAttribute]` můžete uspořádání, řadiče nebo metod, nebo je možné uplatňovat globálně.</span><span class="sxs-lookup"><span data-stu-id="a60a6-149">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="a60a6-150">Použít atribut globálně, přidejte následující kód, který `ConfigureServices` v `Startup`:</span><span class="sxs-lookup"><span data-stu-id="a60a6-150">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="a60a6-151">Předchozí zvýrazněný kód technologie používat všechny požadavky `HTTPS`, proto požadavky HTTP jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="a60a6-151">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="a60a6-152">Následující zvýrazněný kód přesměruje všechny požadavky HTTP na HTTPS:</span><span class="sxs-lookup"><span data-stu-id="a60a6-152">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="a60a6-153">Další informace najdete v tématu [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="a60a6-153">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="a60a6-154">Middleware také umožňuje, že aplikace nastaví stavový kód nebo kód stavu a port, který při spuštění přesměrování.</span><span class="sxs-lookup"><span data-stu-id="a60a6-154">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="a60a6-155">Globálně vyžadování protokolu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) je z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="a60a6-155">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="a60a6-156">Použití `[RequireHttps]` atribut na všechny řadiče/Razor Pages není považován za stejně bezpečné jako globálně vyžadování protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a60a6-156">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="a60a6-157">Nebudete moct zaručit, `[RequireHttps]` atributu se použije při přidání nových řadičů a stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="a60a6-157">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="a60a6-158">Protokol zabezpečení striktní přenos HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="a60a6-158">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="a60a6-159">Za [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [striktní přenos HTTP zabezpečení (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) je vylepšení zabezpečení přihlášení, která je zadána webovou aplikaci pomocí hlavičky speciální odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a60a6-159">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="a60a6-160">Jakmile podporovaný prohlížeč obdrží toto záhlaví prohlížeče zabrání jakékoli komunikační nezabránil odesílání prostřednictvím protokolu HTTP k zadané doméně a místo toho pošle veškerou komunikaci přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a60a6-160">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="a60a6-161">Zabrání také klikněte na protokol HTTPS pomocí pokynů v prohlížečích.</span><span class="sxs-lookup"><span data-stu-id="a60a6-161">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="a60a6-162">Implementuje HSTS s ASP.NET Core 2.1 nebo vyšší `UseHsts` – metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="a60a6-162">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="a60a6-163">Následující kód volá `UseHsts` když aplikace není v [vývojový režim](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="a60a6-163">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="a60a6-164">`UseHsts` není doporučeno při vývoji protože záhlaví HSTS je dokonalé prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="a60a6-164">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="a60a6-165">Ve výchozím nastavení `UseHsts` nezahrnuje adresu místní zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="a60a6-165">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="a60a6-166">Následující kód:</span><span class="sxs-lookup"><span data-stu-id="a60a6-166">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="a60a6-167">Nastaví parametr přednačtení záhlaví zabezpečení přenosu Strict.</span><span class="sxs-lookup"><span data-stu-id="a60a6-167">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="a60a6-168">Předběžné načtení není součástí [specifikaci RFC HSTS](https://tools.ietf.org/html/rfc6797), ale podporuje webových prohlížečů přednačtení HSTS weby na čerstvou instalaci.</span><span class="sxs-lookup"><span data-stu-id="a60a6-168">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="a60a6-169">Zobrazit [ https://hstspreload.org/ ](https://hstspreload.org/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="a60a6-169">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="a60a6-170">Umožňuje [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), které se vztahují zásady HSTS na hostiteli subdomény.</span><span class="sxs-lookup"><span data-stu-id="a60a6-170">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="a60a6-171">Explicitně nastaví parametr max-age záhlaví zabezpečení přenosu Strict na 60 dnů.</span><span class="sxs-lookup"><span data-stu-id="a60a6-171">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="a60a6-172">Pokud není nastavena výchozí hodnota je 30 dní.</span><span class="sxs-lookup"><span data-stu-id="a60a6-172">If not set, defaults to 30 days.</span></span> <span data-ttu-id="a60a6-173">Najdete v článku [max-age směrnice](https://tools.ietf.org/html/rfc6797#section-6.1.1) Další informace.</span><span class="sxs-lookup"><span data-stu-id="a60a6-173">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="a60a6-174">Přidá `example.com` do seznamu hostitelů mají vyloučit.</span><span class="sxs-lookup"><span data-stu-id="a60a6-174">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="a60a6-175">`UseHsts` vyloučí následující zpětné smyčky hostitele:</span><span class="sxs-lookup"><span data-stu-id="a60a6-175">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="a60a6-176">`localhost` : IPv4 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="a60a6-176">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="a60a6-177">`127.0.0.1` : IPv4 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="a60a6-177">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="a60a6-178">`[::1]` : IPv6 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="a60a6-178">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="a60a6-179">Předchozí příklad ukazuje, jak přidat další hostitele.</span><span class="sxs-lookup"><span data-stu-id="a60a6-179">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="a60a6-180">Výslovný nesouhlas s HTTPS při vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="a60a6-180">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="a60a6-181">Šablony ASP.NET Core 2.1 nebo novější webových aplikací (ze sady Visual Studio nebo příkazového řádku dotnet) povolit [HTTPS přesměrování](#require) a [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="a60a6-181">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="a60a6-182">Pro nasazení, které nevyžadují protokol HTTPS je můžete odhlásit protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a60a6-182">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="a60a6-183">Například některé back-endových služeb, kde HTTPS se zpracovává externě na hraničních zařízeních, pomocí protokolu HTTPS v každém uzlu, není nutná.</span><span class="sxs-lookup"><span data-stu-id="a60a6-183">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="a60a6-184">Odhlásit protokolu HTTPS:</span><span class="sxs-lookup"><span data-stu-id="a60a6-184">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a60a6-185">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a60a6-185">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="a60a6-186">Zrušte zaškrtnutí políčka **konfigurace pro protokol HTTPS** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="a60a6-186">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Entity diagram](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a60a6-188">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a60a6-188">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="a60a6-189">Použití `--no-https` možnost.</span><span class="sxs-lookup"><span data-stu-id="a60a6-189">Use the `--no-https` option.</span></span> <span data-ttu-id="a60a6-190">Příklad</span><span class="sxs-lookup"><span data-stu-id="a60a6-190">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="a60a6-191">Jak nastavit certifikát pro vývojáře pro Docker</span><span class="sxs-lookup"><span data-stu-id="a60a6-191">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="a60a6-192">Zobrazit [tento problém Githubu](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="a60a6-192">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
