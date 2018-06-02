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
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="bf6d7-103">Vynutit HTTPS v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bf6d7-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="bf6d7-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bf6d7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bf6d7-105">Tento dokument ukazuje, jak:</span><span class="sxs-lookup"><span data-stu-id="bf6d7-105">This document shows how to:</span></span>

* <span data-ttu-id="bf6d7-106">Vyžadovat protokol HTTPS pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="bf6d7-107">Přesměrujte všechny požadavky HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="bf6d7-108">Proveďte **není** použít [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) na webovým rozhraním API, která zobrazit citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="bf6d7-109">`RequireHttpsAttribute` stavové kódy HTTP se používá k přesměrování prohlížeče z HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="bf6d7-110">Rozhraní API klienti nemusí pochopit a orientují přesměrování z HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="bf6d7-111">Tito klienti mohou odesílat informace přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="bf6d7-112">Webová rozhraní API se musí buď:</span><span class="sxs-lookup"><span data-stu-id="bf6d7-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="bf6d7-113">Nelze naslouchat na protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="bf6d7-114">Ukončení připojení se stavovým kódem 400 (Chybný požadavek) a není požadavek vyřídit.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="bf6d7-115">Vyžadovat protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="bf6d7-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bf6d7-116">Doporučujeme, abyste všechny webové aplikace ASP.NET Core volat Middleware přesměrování protokolu HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) přesměrovat všechny požadavky HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="bf6d7-117">Následující kód volání `UseHttpsRedirection` v `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="bf6d7-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="bf6d7-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="bf6d7-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>

<span data-ttu-id="bf6d7-119">Následující kód volání [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) konfigurace middlewaru možností:</span><span class="sxs-lookup"><span data-stu-id="bf6d7-119">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

<span data-ttu-id="bf6d7-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="bf6d7-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

<span data-ttu-id="bf6d7-121">Předchozí zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="bf6d7-121">The preceding highlighted code:</span></span>

* <span data-ttu-id="bf6d7-122">Nastaví [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode).</span><span class="sxs-lookup"><span data-stu-id="bf6d7-122">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode).</span></span>
* <span data-ttu-id="bf6d7-123">Nastaví HTTPS port 5001.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-123">Sets the HTTPS port to 5001.</span></span>

<span data-ttu-id="bf6d7-124">Automaticky nastavit port následujících mechanismů:</span><span class="sxs-lookup"><span data-stu-id="bf6d7-124">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="bf6d7-125">Middleware může zjišťovat porty prostřednictvím [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) při za následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="bf6d7-125">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="bf6d7-126">Kestrel nebo ovladač HTTP.sys používá přímo s koncových bodů HTTPS (platí také pro spuštění aplikace pomocí ladicího programu Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="bf6d7-126">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="bf6d7-127">Pouze **jeden port HTTPS** používá aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-127">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="bf6d7-128">Visual Studio se používá:</span><span class="sxs-lookup"><span data-stu-id="bf6d7-128">Visual Studio is used:</span></span>
  - <span data-ttu-id="bf6d7-129">Služba IIS Express má povolen protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-129">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="bf6d7-130">*launchSettings.json* nastaví `sslPort` pro službu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-130">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="bf6d7-131">Pokud aplikace běží a za reverzní proxy server (například služby IIS, služba IIS Express), `IServerAddressesFeature` není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-131">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="bf6d7-132">Port musí být ručně nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-132">The port must be manually configured.</span></span> <span data-ttu-id="bf6d7-133">Pokud není nastavený port, požadavky nejsou přesměrovat.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-133">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="bf6d7-134">Port můžete nakonfigurovat pomocí nastavení:</span><span class="sxs-lookup"><span data-stu-id="bf6d7-134">The port can be configured by setting the:</span></span>

* <span data-ttu-id="bf6d7-135">`ASPNETCORE_HTTPS_PORT` Proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-135">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="bf6d7-136">`http_port` Klíč konfigurace hostitele (například prostřednictvím *hostsettings.json* nebo argument příkazového řádku).</span><span class="sxs-lookup"><span data-stu-id="bf6d7-136">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="bf6d7-137">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="bf6d7-137">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="bf6d7-138">Najdete v předchozím příkladu, který ukazuje, jak nastavit port na 5001.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-138">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="bf6d7-139">Port se dá nakonfigurovat nepřímo nastavování adresy URL se `ASPNETCORE_URLS` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-139">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="bf6d7-140">Proměnné prostředí nakonfiguruje server a pak middleware nepřímo zjišťuje port HTTPS přes `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-140">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="bf6d7-141">Pokud je nastaven žádný port:</span><span class="sxs-lookup"><span data-stu-id="bf6d7-141">If no port is set:</span></span>

* <span data-ttu-id="bf6d7-142">Požadavky nejsou přesměrovat.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-142">Requests aren't redirected.</span></span>
* <span data-ttu-id="bf6d7-143">Middleware zaznamená upozornění.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-143">The middleware logs a warning.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="bf6d7-144">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se používá k vyžadují protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-144">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="bf6d7-145">`[RequireHttpsAttribute]` můžete uspořádání řadiče nebo metod, nebo mohou být použity globálně.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-145">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="bf6d7-146">Použít atribut globálně, přidejte následující kód do `ConfigureServices` v `Startup`:</span><span class="sxs-lookup"><span data-stu-id="bf6d7-146">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="bf6d7-147">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="bf6d7-147">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="bf6d7-148">Předchozí zvýrazněný vyžaduje všechny požadavky používat `HTTPS`; proto požadavky HTTP jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-148">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="bf6d7-149">Následující zvýrazněný kód přesměruje všech požadavků HTTP do HTTPS:</span><span class="sxs-lookup"><span data-stu-id="bf6d7-149">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="bf6d7-150">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="bf6d7-150">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="bf6d7-151">Další informace najdete v tématu [URL přepisování Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="bf6d7-151">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="bf6d7-152">Globálně vyžadujících protokol HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) je nejlepším postupem zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-152">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="bf6d7-153">Použití `[RequireHttps]` atribut na všechny řadiče nebo Razor stránky se nepovažuje za bezpečnou jako jako globální vyžadujících protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-153">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="bf6d7-154">Nebudete moct zaručit `[RequireHttps]` atribut se používá, když se přidají nové řadiče a stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-154">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="bf6d7-155">Protokol zabezpečení striktní přenos HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="bf6d7-155">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="bf6d7-156">Za [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [striktní přenos HTTP zabezpečení (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) je určený parametrem webové aplikace prostřednictvím hlavičky odpovědi speciální vylepšení zabezpečení opt-in.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-156">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="bf6d7-157">Jakmile podporovaného prohlížeče obdrží tuto hlavičku prohlížeč tohoto zabrání veškerou komunikaci odesílání prostřednictvím protokolu HTTP k zadané doméně a místo toho odešle veškerou komunikaci přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-157">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="bf6d7-158">Rovněž zamezí klikněte na protokol HTTPS přes výzvy v prohlížečích.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-158">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="bf6d7-159">Implementuje HSTS s ASP.NET Core 2.1 nebo vyšší `UseHsts` metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-159">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="bf6d7-160">Následující kód volání `UseHsts` Pokud aplikace není v [režimu pro vývoj](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="bf6d7-160">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="bf6d7-161">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="bf6d7-161">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="bf6d7-162">`UseHsts` není doporučujeme při vývoji protože záhlaví HSTS je vysoce cachable pomocí prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-162">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="bf6d7-163">Ve výchozím nastavení vyloučí UseHsts adresu místní smyčky.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-163">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="bf6d7-164">Následující kód:</span><span class="sxs-lookup"><span data-stu-id="bf6d7-164">The following code:</span></span>

<span data-ttu-id="bf6d7-165">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="bf6d7-165">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="bf6d7-166">Nastaví parametr přednačtení záhlaví zabezpečení přenosu Strict.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-166">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="bf6d7-167">Předběžného načítání není součástí [specifikaci RFC HSTS](https://tools.ietf.org/html/rfc6797), ale podporuje webových prohlížečů přednačtení HSTS weby na čerstvou instalaci.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-167">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="bf6d7-168">V tématu [ https://hstspreload.org/ ](https://hstspreload.org/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-168">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="bf6d7-169">Umožňuje [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), které se vztahují zásady HSTS na hostiteli subdomény.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-169">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="bf6d7-170">Explicitně nastaví parametr maximální stáří zabezpečení přenosu Strict hlavičky k na 60 dnů.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-170">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="bf6d7-171">Pokud není nastavena, výchozí hodnota je 30 dní.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-171">If not set, defaults to 30 days.</span></span> <span data-ttu-id="bf6d7-172">Najdete v článku [maximální stáří direktiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) Další informace.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-172">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="bf6d7-173">Přidá `example.com` do seznamu hostitelů, které chcete vyloučit.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-173">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="bf6d7-174">`UseHsts` vyloučí následující zpětné smyčky hostitele:</span><span class="sxs-lookup"><span data-stu-id="bf6d7-174">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="bf6d7-175">`localhost` : IPv4 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-175">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="bf6d7-176">`127.0.0.1` : IPv4 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-176">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="bf6d7-177">`[::1]` : IPv6 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-177">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="bf6d7-178">Předchozí příklad ukazuje, jak přidat další hostitele.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-178">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="bf6d7-179">Výslovný nesouhlas s HTTPS na vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="bf6d7-179">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="bf6d7-180">Povolit šablony ASP.NET Core 2.1 nebo novější webových aplikací (ze sady Visual Studio nebo pomocí příkazového řádku dotnet) [HTTPS přesměrování](#require) a [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="bf6d7-180">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="bf6d7-181">Pro nasazení, které nevyžadují protokol HTTPS které můžete výslovný nesouhlas s protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-181">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="bf6d7-182">Například není potřeba některé služby back-end, kde HTTPS je zpracovanou externě na okraji a pomocí protokolu HTTPS v každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-182">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="bf6d7-183">Vyjádřit výslovný nesouhlas protokolu HTTPS:</span><span class="sxs-lookup"><span data-stu-id="bf6d7-183">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bf6d7-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bf6d7-184">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="bf6d7-185">Zrušte zaškrtnutí políčka **konfigurace pro protokol HTTPS** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-185">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Entity diagram](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bf6d7-187">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="bf6d7-187">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="bf6d7-188">Použití `--no-https` možnost.</span><span class="sxs-lookup"><span data-stu-id="bf6d7-188">Use the `--no-https` option.</span></span> <span data-ttu-id="bf6d7-189">Příklad</span><span class="sxs-lookup"><span data-stu-id="bf6d7-189">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="bf6d7-190">Postup instalace certifikátu vývojáře pro Docker</span><span class="sxs-lookup"><span data-stu-id="bf6d7-190">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="bf6d7-191">V tématu [potíže Githubu](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="bf6d7-191">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
