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
ms.openlocfilehash: f49a7846149385125390285e2f1332d8e40642c0
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/18/2018
ms.locfileid: "35725933"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="dbfe7-103">Vynutit HTTPS v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dbfe7-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="dbfe7-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dbfe7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dbfe7-105">Tento dokument ukazuje, jak:</span><span class="sxs-lookup"><span data-stu-id="dbfe7-105">This document shows how to:</span></span>

* <span data-ttu-id="dbfe7-106">Vyžadovat protokol HTTPS pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="dbfe7-107">Přesměrujte všechny požadavky HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="dbfe7-108">Proveďte **není** použít [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) na webovým rozhraním API, která zobrazit citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="dbfe7-109">`RequireHttpsAttribute` stavové kódy HTTP se používá k přesměrování prohlížeče z HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="dbfe7-110">Rozhraní API klienti nemusí pochopit a orientují přesměrování z HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="dbfe7-111">Tito klienti mohou odesílat informace přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="dbfe7-112">Webová rozhraní API se musí buď:</span><span class="sxs-lookup"><span data-stu-id="dbfe7-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="dbfe7-113">Nelze naslouchat na protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="dbfe7-114">Ukončení připojení se stavovým kódem 400 (Chybný požadavek) a není požadavek vyřídit.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="dbfe7-115">Vyžadovat protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="dbfe7-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="dbfe7-116">Doporučujeme, abyste všechny webové aplikace ASP.NET Core volat Middleware přesměrování protokolu HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) přesměrovat všechny požadavky HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="dbfe7-117">Následující kód volání `UseHttpsRedirection` v `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="dbfe7-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="dbfe7-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="dbfe7-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>

<span data-ttu-id="dbfe7-119">Následující kód volání [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) konfigurace middlewaru možností:</span><span class="sxs-lookup"><span data-stu-id="dbfe7-119">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

<span data-ttu-id="dbfe7-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="dbfe7-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

<span data-ttu-id="dbfe7-121">Předchozí zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="dbfe7-121">The preceding highlighted code:</span></span>

* <span data-ttu-id="dbfe7-122">Nastaví [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) k `Status307TemporaryRedirect`, což je výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-122">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="dbfe7-123">Produkční aplikace by měly volat [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="dbfe7-123">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="dbfe7-124">Nastaví HTTPS port 5001.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-124">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="dbfe7-125">Výchozí hodnota je 443.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-125">The default value is 443.</span></span>

<span data-ttu-id="dbfe7-126">Automaticky nastavit port následujících mechanismů:</span><span class="sxs-lookup"><span data-stu-id="dbfe7-126">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="dbfe7-127">Middleware může zjišťovat porty prostřednictvím [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) při za následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="dbfe7-127">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="dbfe7-128">Kestrel nebo ovladač HTTP.sys používá přímo s koncových bodů HTTPS (platí také pro spuštění aplikace pomocí ladicího programu Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="dbfe7-128">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="dbfe7-129">Pouze **jeden port HTTPS** používá aplikace.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-129">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="dbfe7-130">Visual Studio se používá:</span><span class="sxs-lookup"><span data-stu-id="dbfe7-130">Visual Studio is used:</span></span>
  - <span data-ttu-id="dbfe7-131">Služba IIS Express má povolen protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-131">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="dbfe7-132">*launchSettings.json* nastaví `sslPort` pro službu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-132">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="dbfe7-133">Pokud aplikace běží a za reverzní proxy server (například služby IIS, služba IIS Express), `IServerAddressesFeature` není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-133">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="dbfe7-134">Port musí být ručně nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-134">The port must be manually configured.</span></span> <span data-ttu-id="dbfe7-135">Pokud není nastavený port, požadavky nejsou přesměrovat.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-135">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="dbfe7-136">Port můžete nakonfigurovat pomocí nastavení:</span><span class="sxs-lookup"><span data-stu-id="dbfe7-136">The port can be configured by setting the:</span></span>

* <span data-ttu-id="dbfe7-137">`ASPNETCORE_HTTPS_PORT` Proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-137">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="dbfe7-138">`http_port` Klíč konfigurace hostitele (například prostřednictvím *hostsettings.json* nebo argument příkazového řádku).</span><span class="sxs-lookup"><span data-stu-id="dbfe7-138">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="dbfe7-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="dbfe7-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="dbfe7-140">Najdete v předchozím příkladu, který ukazuje, jak nastavit port na 5001.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-140">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="dbfe7-141">Port se dá nakonfigurovat nepřímo nastavování adresy URL se `ASPNETCORE_URLS` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="dbfe7-142">Proměnné prostředí nakonfiguruje server a pak middleware nepřímo zjišťuje port HTTPS přes `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="dbfe7-143">Pokud je nastaven žádný port:</span><span class="sxs-lookup"><span data-stu-id="dbfe7-143">If no port is set:</span></span>

* <span data-ttu-id="dbfe7-144">Požadavky nejsou přesměrovat.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="dbfe7-145">Middleware zaznamená upozornění.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="dbfe7-146">Alternativu k použití protokolu HTTPS přesměrování Middleware (`UseHttpsRedirection`), je použít Middleware přepisování adresy URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="dbfe7-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="dbfe7-147">`AddRedirectToHttps` Můžete také nastavit stavový kód a portu při přesměrování.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="dbfe7-148">Další informace najdete v tématu [URL přepisování Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="dbfe7-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="dbfe7-149">Při přesměrování na HTTPS bez nutnosti další přesměrování pravidla, doporučujeme používat protokol HTTPS přesměrování Middleware (`UseHttpsRedirection`) popsané v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="dbfe7-150">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se používá k vyžadují protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="dbfe7-151">`[RequireHttpsAttribute]` můžete uspořádání řadiče nebo metod, nebo mohou být použity globálně.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="dbfe7-152">Použít atribut globálně, přidejte následující kód do `ConfigureServices` v `Startup`:</span><span class="sxs-lookup"><span data-stu-id="dbfe7-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="dbfe7-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="dbfe7-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="dbfe7-154">Předchozí zvýrazněný vyžaduje všechny požadavky používat `HTTPS`; proto požadavky HTTP jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-154">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="dbfe7-155">Následující zvýrazněný kód přesměruje všech požadavků HTTP do HTTPS:</span><span class="sxs-lookup"><span data-stu-id="dbfe7-155">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="dbfe7-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="dbfe7-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="dbfe7-157">Další informace najdete v tématu [URL přepisování Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="dbfe7-157">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="dbfe7-158">Middleware taky umožňuje aplikaci a nastavte stavový kód nebo kód stavu a port, když se spustí přesměrování.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-158">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="dbfe7-159">Globálně vyžadujících protokol HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) je nejlepším postupem zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-159">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="dbfe7-160">Použití `[RequireHttps]` atribut na všechny řadiče nebo Razor stránky se nepovažuje za bezpečnou jako jako globální vyžadujících protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-160">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="dbfe7-161">Nebudete moct zaručit `[RequireHttps]` atribut se používá, když se přidají nové řadiče a stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-161">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="dbfe7-162">Protokol zabezpečení striktní přenos HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="dbfe7-162">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="dbfe7-163">Za [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [striktní přenos HTTP zabezpečení (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) je určený parametrem webové aplikace prostřednictvím hlavičky odpovědi speciální vylepšení zabezpečení opt-in.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-163">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="dbfe7-164">Jakmile podporovaného prohlížeče obdrží tuto hlavičku prohlížeč tohoto zabrání veškerou komunikaci odesílání prostřednictvím protokolu HTTP k zadané doméně a místo toho odešle veškerou komunikaci přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-164">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="dbfe7-165">Rovněž zamezí klikněte na protokol HTTPS přes výzvy v prohlížečích.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-165">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="dbfe7-166">Implementuje HSTS s ASP.NET Core 2.1 nebo vyšší `UseHsts` metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-166">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="dbfe7-167">Následující kód volání `UseHsts` Pokud aplikace není v [režimu pro vývoj](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="dbfe7-167">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="dbfe7-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="dbfe7-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="dbfe7-169">`UseHsts` není doporučeno, vývojem protože záhlaví HSTS je vysoce lze uložit do mezipaměti prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-169">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="dbfe7-170">Ve výchozím nastavení `UseHsts` vyloučí adresu místní smyčky.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-170">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="dbfe7-171">Následující kód:</span><span class="sxs-lookup"><span data-stu-id="dbfe7-171">The following code:</span></span>

<span data-ttu-id="dbfe7-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="dbfe7-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="dbfe7-173">Nastaví parametr přednačtení záhlaví zabezpečení přenosu Strict.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-173">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="dbfe7-174">Předběžného načítání není součástí [specifikaci RFC HSTS](https://tools.ietf.org/html/rfc6797), ale podporuje webových prohlížečů přednačtení HSTS weby na čerstvou instalaci.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-174">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="dbfe7-175">V tématu [ https://hstspreload.org/ ](https://hstspreload.org/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-175">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="dbfe7-176">Umožňuje [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), které se vztahují zásady HSTS na hostiteli subdomény.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-176">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="dbfe7-177">Explicitně nastaví parametr maximální stáří zabezpečení přenosu Strict hlavičky k na 60 dnů.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-177">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="dbfe7-178">Pokud není nastavena, výchozí hodnota je 30 dní.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-178">If not set, defaults to 30 days.</span></span> <span data-ttu-id="dbfe7-179">Najdete v článku [maximální stáří direktiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) Další informace.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-179">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="dbfe7-180">Přidá `example.com` do seznamu hostitelů, které chcete vyloučit.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-180">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="dbfe7-181">`UseHsts` vyloučí následující zpětné smyčky hostitele:</span><span class="sxs-lookup"><span data-stu-id="dbfe7-181">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="dbfe7-182">`localhost` : IPv4 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-182">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="dbfe7-183">`127.0.0.1` : IPv4 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-183">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="dbfe7-184">`[::1]` : IPv6 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-184">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="dbfe7-185">Předchozí příklad ukazuje, jak přidat další hostitele.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-185">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="dbfe7-186">Výslovný nesouhlas s HTTPS na vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="dbfe7-186">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="dbfe7-187">Povolit šablony ASP.NET Core 2.1 nebo novější webových aplikací (ze sady Visual Studio nebo pomocí příkazového řádku dotnet) [HTTPS přesměrování](#require) a [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="dbfe7-187">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="dbfe7-188">Pro nasazení, které nevyžadují protokol HTTPS které můžete výslovný nesouhlas s protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-188">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="dbfe7-189">Například není potřeba některé služby back-end, kde HTTPS je zpracovanou externě na okraji a pomocí protokolu HTTPS v každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-189">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="dbfe7-190">Vyjádřit výslovný nesouhlas protokolu HTTPS:</span><span class="sxs-lookup"><span data-stu-id="dbfe7-190">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dbfe7-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dbfe7-191">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="dbfe7-192">Zrušte zaškrtnutí políčka **konfigurace pro protokol HTTPS** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-192">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Entity diagram](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="dbfe7-194">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="dbfe7-194">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="dbfe7-195">Použití `--no-https` možnost.</span><span class="sxs-lookup"><span data-stu-id="dbfe7-195">Use the `--no-https` option.</span></span> <span data-ttu-id="dbfe7-196">Příklad</span><span class="sxs-lookup"><span data-stu-id="dbfe7-196">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="dbfe7-198">Postup instalace certifikátu vývojáře pro Docker</span><span class="sxs-lookup"><span data-stu-id="dbfe7-198">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="dbfe7-199">V tématu [potíže Githubu](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="dbfe7-199">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
