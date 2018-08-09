---
title: Vynucení protokolu HTTPS v ASP.NET Core
author: rick-anderson
description: Ukazuje, jak chcete vyžadovat protokol HTTPS/TLS v ASP.NET Core webové aplikace.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 3bea8661e17fec5128e822d98741d1f8ed7434e5
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655495"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="bc686-103">Vynucení protokolu HTTPS v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bc686-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="bc686-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bc686-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bc686-105">Tento dokument ukazuje, jak:</span><span class="sxs-lookup"><span data-stu-id="bc686-105">This document shows how to:</span></span>

* <span data-ttu-id="bc686-106">Vyžadovat protokol HTTPS pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="bc686-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="bc686-107">Přesměrujte všechny požadavky HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bc686-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="bc686-108">Proveďte **není** použít [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) na webová rozhraní API, která zobrazit citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="bc686-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="bc686-109">`RequireHttpsAttribute` stavové kódy HTTP se používá pro přesměrování prohlížeče z HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bc686-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="bc686-110">Klienti rozhraní API nemusí pochopit a dodržují přesměrování z HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bc686-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="bc686-111">Tito klienti mohou odesílat informace přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc686-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="bc686-112">Webová rozhraní API by buď:</span><span class="sxs-lookup"><span data-stu-id="bc686-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="bc686-113">Nelze naslouchat na HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc686-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="bc686-114">Ukončete připojení s stavový kód 400 (Chybný požadavek) a není obsluhovat žádosti.</span><span class="sxs-lookup"><span data-stu-id="bc686-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="bc686-115">Vyžadovat protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="bc686-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bc686-116">Doporučujeme vám, že všechny webové aplikace ASP.NET Core volat Middleware přesměrování protokolu HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) přesměrovat všechny požadavky HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bc686-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="bc686-117">Následující kód volá `UseHttpsRedirection` v `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="bc686-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="bc686-118">Předchozí zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="bc686-118">The preceding highlighted code:</span></span>

* <span data-ttu-id="bc686-119">Používá výchozí [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="bc686-119">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span> <span data-ttu-id="bc686-120">Produkční aplikace by měly volat [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="bc686-120">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="bc686-121">Používá výchozí [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span><span class="sxs-lookup"><span data-stu-id="bc686-121">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span></span>

<span data-ttu-id="bc686-122">Následující kód volá [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) konfigurace middlewaru možností:</span><span class="sxs-lookup"><span data-stu-id="bc686-122">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="bc686-123">Předchozí zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="bc686-123">The preceding highlighted code:</span></span>

* <span data-ttu-id="bc686-124">Nastaví [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) k `Status307TemporaryRedirect`, což je výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="bc686-124">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="bc686-125">Produkční aplikace by měly volat [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="bc686-125">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="bc686-126">Nastaví HTTPS port 5001.</span><span class="sxs-lookup"><span data-stu-id="bc686-126">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="bc686-127">Výchozí hodnota je 443.</span><span class="sxs-lookup"><span data-stu-id="bc686-127">The default value is 443.</span></span>

<span data-ttu-id="bc686-128">Automaticky nastavit port následujících mechanismů:</span><span class="sxs-lookup"><span data-stu-id="bc686-128">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="bc686-129">Middleware najdou porty prostřednictvím [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) když platí tyto podmínky:</span><span class="sxs-lookup"><span data-stu-id="bc686-129">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="bc686-130">Kestrel nebo ovladač HTTP.sys se používá přímo s koncovým bodům protokol HTTPS (platí také pro spuštění aplikace pomocí ladicího programu nástroje Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="bc686-130">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="bc686-131">Pouze **jeden port HTTPS** používají aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bc686-131">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="bc686-132">Visual Studio se používá:</span><span class="sxs-lookup"><span data-stu-id="bc686-132">Visual Studio is used:</span></span>
  - <span data-ttu-id="bc686-133">Služba IIS Express má povolený protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bc686-133">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="bc686-134">*launchSettings.json* nastaví `sslPort` pro službu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="bc686-134">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="bc686-135">Při spuštění aplikace za reverzní proxy server (například služby IIS, IIS Express), `IServerAddressesFeature` není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="bc686-135">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="bc686-136">Port musí být ručně nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="bc686-136">The port must be manually configured.</span></span> <span data-ttu-id="bc686-137">Pokud port není nastavena, přesměrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="bc686-137">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="bc686-138">Port, který se dá nakonfigurovat pomocí nastavení [nastavení konfigurace webového hostitele https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="bc686-138">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="bc686-139">**Klíč**: https_port **typ**: *řetězec*
**výchozí**: výchozí hodnota není nastavená.</span><span class="sxs-lookup"><span data-stu-id="bc686-139">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="bc686-140">**Sada s použitím**: `UseSetting` 
 **proměnnou prostředí**: `<PREFIX_>HTTPS_PORT` (předpona je `ASPNETCORE_` při použití webového hostitele.)</span><span class="sxs-lookup"><span data-stu-id="bc686-140">**Set using**: `UseSetting`
**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="bc686-141">Port, který se dá nakonfigurovat nepřímo pomocí nastavení adresy URL s `ASPNETCORE_URLS` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="bc686-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="bc686-142">Proměnná prostředí nakonfiguruje server a pak middleware nepřímo zjistí port HTTPS přes `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="bc686-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="bc686-143">Pokud je nastaven žádný port:</span><span class="sxs-lookup"><span data-stu-id="bc686-143">If no port is set:</span></span>

* <span data-ttu-id="bc686-144">Přesměrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="bc686-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="bc686-145">Middleware zaprotokoluje upozornění.</span><span class="sxs-lookup"><span data-stu-id="bc686-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="bc686-146">Alternativou k používání HTTPS přesměrování Middleware (`UseHttpsRedirection`), je použít Middleware pro přepis adres URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="bc686-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="bc686-147">`AddRedirectToHttps` Můžete také nastavit stavový kód a portu při spuštění přesměrování.</span><span class="sxs-lookup"><span data-stu-id="bc686-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="bc686-148">Další informace najdete v tématu [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="bc686-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="bc686-149">Při přesměrování na HTTPS bez nutnosti další přesměrování pravidla, doporučujeme používat Middleware přesměrování protokolu HTTPS (`UseHttpsRedirection`) popsané v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="bc686-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="bc686-150">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se používá tak, aby vyžadovala protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bc686-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="bc686-151">`[RequireHttpsAttribute]` můžete uspořádání, řadiče nebo metod, nebo je možné uplatňovat globálně.</span><span class="sxs-lookup"><span data-stu-id="bc686-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="bc686-152">Použít atribut globálně, přidejte následující kód, který `ConfigureServices` v `Startup`:</span><span class="sxs-lookup"><span data-stu-id="bc686-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="bc686-153">Předchozí zvýrazněný kód technologie používat všechny požadavky `HTTPS`, proto požadavky HTTP jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="bc686-153">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="bc686-154">Následující zvýrazněný kód přesměruje všechny požadavky HTTP na HTTPS:</span><span class="sxs-lookup"><span data-stu-id="bc686-154">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="bc686-155">Další informace najdete v tématu [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="bc686-155">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="bc686-156">Middleware také umožňuje, že aplikace nastaví stavový kód nebo kód stavu a port, který při spuštění přesměrování.</span><span class="sxs-lookup"><span data-stu-id="bc686-156">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="bc686-157">Globálně vyžadování protokolu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) je z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="bc686-157">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="bc686-158">Použití `[RequireHttps]` atribut na všechny řadiče/Razor Pages není považován za stejně bezpečné jako globálně vyžadování protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bc686-158">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="bc686-159">Nebudete moct zaručit, `[RequireHttps]` atributu se použije při přidání nových řadičů a stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="bc686-159">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="bc686-160">Protokol zabezpečení striktní přenos HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="bc686-160">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="bc686-161">Za [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [striktní přenos HTTP zabezpečení (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) je vylepšení zabezpečení přihlášení, která je zadána pomocí hlavičky odpovědi webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="bc686-161">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="bc686-162">Když prohlížeč, který podporuje HSTS obdrží toto záhlaví:</span><span class="sxs-lookup"><span data-stu-id="bc686-162">When a browser that supports HSTS receives this header:</span></span>

* <span data-ttu-id="bc686-163">Prohlížeč ukládá konfiguraci pro doménu, která zabrání odeslání libovolné komunikaci přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc686-163">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="bc686-164">Prohlížeč vynutí veškerou komunikaci přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bc686-164">The browser forces all communication over HTTPS.</span></span> 
* <span data-ttu-id="bc686-165">Prohlížeč znemožní uživateli pomocí certifikátů nedůvěryhodný nebo neplatný.</span><span class="sxs-lookup"><span data-stu-id="bc686-165">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="bc686-166">Prohlížeč zakáže výzev, které umožní uživateli tohoto certifikátu dočasně důvěřovat.</span><span class="sxs-lookup"><span data-stu-id="bc686-166">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="bc686-167">Implementuje HSTS s ASP.NET Core 2.1 nebo vyšší `UseHsts` – metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bc686-167">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="bc686-168">Následující kód volá `UseHsts` když aplikace není v [vývojový režim](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="bc686-168">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="bc686-169">`UseHsts` není doporučeno při vývoji protože záhlaví HSTS je dokonalé prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="bc686-169">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="bc686-170">Ve výchozím nastavení `UseHsts` nezahrnuje adresu místní zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="bc686-170">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="bc686-171">Pro produkční prostředí implementace HTTPS poprvé nastavte počáteční hodnotu HSTS malou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc686-171">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="bc686-172">Nastavte hodnotu hodiny na no více než jeden den v případě, že budete potřebovat obnovit infrastruktury HTTPS do HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc686-172">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="bc686-173">Poté, co jste si jisti udržitelnost konfigurace protokolu HTTPS, zvýšit hodnotu max-age HSTS. běžně používané hodnota je 1 rok.</span><span class="sxs-lookup"><span data-stu-id="bc686-173">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="bc686-174">Následující kód:</span><span class="sxs-lookup"><span data-stu-id="bc686-174">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="bc686-175">Nastaví parametr přednačtení záhlaví zabezpečení přenosu Strict.</span><span class="sxs-lookup"><span data-stu-id="bc686-175">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="bc686-176">Předběžné načtení není součástí [specifikaci RFC HSTS](https://tools.ietf.org/html/rfc6797), ale podporuje webových prohlížečů přednačtení HSTS weby na čerstvou instalaci.</span><span class="sxs-lookup"><span data-stu-id="bc686-176">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="bc686-177">Zobrazit [ https://hstspreload.org/ ](https://hstspreload.org/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="bc686-177">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="bc686-178">Umožňuje [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), které se vztahují zásady HSTS na hostiteli subdomény.</span><span class="sxs-lookup"><span data-stu-id="bc686-178">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="bc686-179">Explicitně nastaví parametr max-age záhlaví zabezpečení přenosu Strict na 60 dnů.</span><span class="sxs-lookup"><span data-stu-id="bc686-179">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="bc686-180">Pokud není nastavena výchozí hodnota je 30 dní.</span><span class="sxs-lookup"><span data-stu-id="bc686-180">If not set, defaults to 30 days.</span></span> <span data-ttu-id="bc686-181">Najdete v článku [max-age směrnice](https://tools.ietf.org/html/rfc6797#section-6.1.1) Další informace.</span><span class="sxs-lookup"><span data-stu-id="bc686-181">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="bc686-182">Přidá `example.com` do seznamu hostitelů mají vyloučit.</span><span class="sxs-lookup"><span data-stu-id="bc686-182">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="bc686-183">`UseHsts` vyloučí následující zpětné smyčky hostitele:</span><span class="sxs-lookup"><span data-stu-id="bc686-183">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="bc686-184">`localhost` : IPv4 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="bc686-184">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="bc686-185">`127.0.0.1` : IPv4 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="bc686-185">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="bc686-186">`[::1]` : IPv6 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="bc686-186">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="bc686-187">Předchozí příklad ukazuje, jak přidat další hostitele.</span><span class="sxs-lookup"><span data-stu-id="bc686-187">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="bc686-188">Výslovný nesouhlas s HTTPS při vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="bc686-188">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="bc686-189">Šablony ASP.NET Core 2.1 nebo novější webových aplikací (ze sady Visual Studio nebo příkazového řádku dotnet) povolit [HTTPS přesměrování](#require) a [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="bc686-189">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="bc686-190">Pro nasazení, které nevyžadují protokol HTTPS je můžete odhlásit protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bc686-190">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="bc686-191">Například některé back-endových služeb, kde HTTPS se zpracovává externě na hraničních zařízeních, pomocí protokolu HTTPS v každém uzlu, není nutná.</span><span class="sxs-lookup"><span data-stu-id="bc686-191">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="bc686-192">Odhlásit protokolu HTTPS:</span><span class="sxs-lookup"><span data-stu-id="bc686-192">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bc686-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc686-193">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="bc686-194">Zrušte zaškrtnutí políčka **konfigurace pro protokol HTTPS** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="bc686-194">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Entity diagram](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bc686-196">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="bc686-196">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="bc686-197">Použití `--no-https` možnost.</span><span class="sxs-lookup"><span data-stu-id="bc686-197">Use the `--no-https` option.</span></span> <span data-ttu-id="bc686-198">Příklad</span><span class="sxs-lookup"><span data-stu-id="bc686-198">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="bc686-199">Jak nastavit certifikát pro vývojáře pro Docker</span><span class="sxs-lookup"><span data-stu-id="bc686-199">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="bc686-200">Zobrazit [tento problém Githubu](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="bc686-200">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
