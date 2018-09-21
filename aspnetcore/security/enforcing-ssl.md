---
title: Vynucení protokolu HTTPS v ASP.NET Core
author: rick-anderson
description: Ukazuje, jak chcete vyžadovat protokol HTTPS/TLS v ASP.NET Core webové aplikace.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 6e16191b1a4627e683fd2281e5556b2a6e84c082
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523139"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="7ab33-103">Vynucení protokolu HTTPS v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ab33-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="7ab33-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7ab33-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7ab33-105">Tento dokument ukazuje, jak:</span><span class="sxs-lookup"><span data-stu-id="7ab33-105">This document shows how to:</span></span>

* <span data-ttu-id="7ab33-106">Vyžadovat protokol HTTPS pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="7ab33-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="7ab33-107">Přesměrujte všechny požadavky HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ab33-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="7ab33-108">Žádné rozhraní API můžete zabránit posláním citlivých dat na první požadavek klienta.</span><span class="sxs-lookup"><span data-stu-id="7ab33-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="7ab33-109">Proveďte **není** použít [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) na webová rozhraní API, která zobrazit citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="7ab33-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="7ab33-110">`RequireHttpsAttribute` stavové kódy HTTP se používá pro přesměrování prohlížeče z HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ab33-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="7ab33-111">Klienti rozhraní API nemusí pochopit a dodržují přesměrování z HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ab33-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="7ab33-112">Tito klienti mohou odesílat informace přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="7ab33-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="7ab33-113">Webová rozhraní API by buď:</span><span class="sxs-lookup"><span data-stu-id="7ab33-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="7ab33-114">Nelze naslouchat na HTTP.</span><span class="sxs-lookup"><span data-stu-id="7ab33-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="7ab33-115">Ukončete připojení s stavový kód 400 (Chybný požadavek) a není obsluhovat žádosti.</span><span class="sxs-lookup"><span data-stu-id="7ab33-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="7ab33-116">Vyžadovat protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="7ab33-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7ab33-117">Doporučujeme všechny produkční ASP.NET Core volání webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="7ab33-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="7ab33-118">Middleware přesměrování protokolu HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) přesměrovat všechny požadavky HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ab33-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="7ab33-119">[UseHsts](#hsts), přenosový striktní bezpečnostní protokol HTTP (HSTS).</span><span class="sxs-lookup"><span data-stu-id="7ab33-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="7ab33-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="7ab33-120">UseHttpsRedirection</span></span>

<span data-ttu-id="7ab33-121">Následující kód volá `UseHttpsRedirection` v `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="7ab33-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="7ab33-122">Předchozí zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="7ab33-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="7ab33-123">Používá výchozí [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="7ab33-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span>
* <span data-ttu-id="7ab33-124">Používá výchozí [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), pokud není přepsán `ASPNETCORE_HTTPS_PORT` proměnné prostředí nebo [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="7ab33-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

> [!WARNING] 
><span data-ttu-id="7ab33-125">Port musí být k dispozici pro daný middleware přesměrovat na protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ab33-125">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="7ab33-126">Pokud není port je k dispozici, nedojde k přesměrování na protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ab33-126">If no port is available, redirection to HTTPS does not occur.</span></span> <span data-ttu-id="7ab33-127">HTTPS port se dá nastavit některým z následujících nastavení:</span><span class="sxs-lookup"><span data-stu-id="7ab33-127">The HTTPS port can be specified by any of the following setting:</span></span>
> 
>* `HttpsRedirectionOptions.HttpsPort` 
>* <span data-ttu-id="7ab33-128">`ASPNETCORE_HTTPS_PORT` Proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="7ab33-128">The `ASPNETCORE_HTTPS_PORT` environment variable.</span></span> 
>* <span data-ttu-id="7ab33-129">Při vývoji, adresu url HTTPS v *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7ab33-129">In development, an HTTPS url in *launchsettings.json*.</span></span> 
>* <span data-ttu-id="7ab33-130">Adresu url HTTPS nakonfigurovaná přímo na Kestrel nebo HttpSys.</span><span class="sxs-lookup"><span data-stu-id="7ab33-130">An HTTPS url configured directly on Kestrel or HttpSys.</span></span> 

<span data-ttu-id="7ab33-131">Následující zvýrazněný kód volá [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) konfigurace middlewaru možností:</span><span class="sxs-lookup"><span data-stu-id="7ab33-131">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="7ab33-132">Volání `AddHttpsRedirection` jenom je potřeba změnit hodnoty ` HttpsPort` nebo ` RedirectStatusCode`;</span><span class="sxs-lookup"><span data-stu-id="7ab33-132">Calling `AddHttpsRedirection` is only necessary to change the values of ` HttpsPort` or ` RedirectStatusCode`;</span></span>

<span data-ttu-id="7ab33-133">Předchozí zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="7ab33-133">The preceding highlighted code:</span></span>

* <span data-ttu-id="7ab33-134">Nastaví [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) k `Status307TemporaryRedirect`, což je výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="7ab33-134">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span>
* <span data-ttu-id="7ab33-135">Nastaví HTTPS port 5001.</span><span class="sxs-lookup"><span data-stu-id="7ab33-135">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="7ab33-136">Výchozí hodnota je 443.</span><span class="sxs-lookup"><span data-stu-id="7ab33-136">The default value is 443.</span></span>

<span data-ttu-id="7ab33-137">Automaticky nastavit port následujících mechanismů:</span><span class="sxs-lookup"><span data-stu-id="7ab33-137">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="7ab33-138">Middleware najdou porty prostřednictvím [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) když platí tyto podmínky:</span><span class="sxs-lookup"><span data-stu-id="7ab33-138">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

   * <span data-ttu-id="7ab33-139">Kestrel nebo ovladač HTTP.sys se používá přímo s koncovým bodům protokol HTTPS (platí také pro spuštění aplikace pomocí ladicího programu nástroje Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="7ab33-139">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
   * <span data-ttu-id="7ab33-140">Pouze **jeden port HTTPS** používají aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7ab33-140">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="7ab33-141">Visual Studio se používá:</span><span class="sxs-lookup"><span data-stu-id="7ab33-141">Visual Studio is used:</span></span>
   * <span data-ttu-id="7ab33-142">Služba IIS Express má povolený protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ab33-142">IIS Express has HTTPS enabled.</span></span>
   * <span data-ttu-id="7ab33-143">*launchSettings.json* nastaví `sslPort` pro službu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="7ab33-143">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="7ab33-144">Při spuštění aplikace za reverzní proxy server (například služby IIS, IIS Express), `IServerAddressesFeature` není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="7ab33-144">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="7ab33-145">Port musí být ručně nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="7ab33-145">The port must be manually configured.</span></span> <span data-ttu-id="7ab33-146">Pokud port není nastavena, přesměrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="7ab33-146">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="7ab33-147">Port, který se dá nakonfigurovat pomocí nastavení [nastavení konfigurace webového hostitele https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="7ab33-147">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="7ab33-148">**Klíč**: https_port</span><span class="sxs-lookup"><span data-stu-id="7ab33-148">**Key**: https_port</span></span>  
<span data-ttu-id="7ab33-149">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="7ab33-149">**Type**: *string*</span></span>  
<span data-ttu-id="7ab33-150">**Výchozí**: výchozí hodnota není nastavená.</span><span class="sxs-lookup"><span data-stu-id="7ab33-150">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="7ab33-151">**Sada s použitím**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="7ab33-151">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="7ab33-152">**Proměnná prostředí**: `<PREFIX_>HTTPS_PORT` (předpona je `ASPNETCORE_` při použití webového hostitele.)</span><span class="sxs-lookup"><span data-stu-id="7ab33-152">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="7ab33-153">Port, který se dá nakonfigurovat nepřímo pomocí nastavení adresy URL s `ASPNETCORE_URLS` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="7ab33-153">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="7ab33-154">Proměnná prostředí nakonfiguruje server a pak middleware nepřímo zjistí port HTTPS přes `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="7ab33-154">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="7ab33-155">Pokud je nastaven žádný port:</span><span class="sxs-lookup"><span data-stu-id="7ab33-155">If no port is set:</span></span>

* <span data-ttu-id="7ab33-156">Přesměrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="7ab33-156">Requests aren't redirected.</span></span>
* <span data-ttu-id="7ab33-157">Middleware protokoluje upozornění "Nepovedlo se určit port https pro přesměrování."</span><span class="sxs-lookup"><span data-stu-id="7ab33-157">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="7ab33-158">Alternativou k používání HTTPS přesměrování Middleware (`UseHttpsRedirection`), je použít Middleware pro přepis adres URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="7ab33-158">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="7ab33-159">`AddRedirectToHttps` Můžete také nastavit stavový kód a portu při spuštění přesměrování.</span><span class="sxs-lookup"><span data-stu-id="7ab33-159">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="7ab33-160">Další informace najdete v tématu [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="7ab33-160">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="7ab33-161">Při přesměrování na HTTPS bez nutnosti další přesměrování pravidla, doporučujeme používat Middleware přesměrování protokolu HTTPS (`UseHttpsRedirection`) popsané v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="7ab33-161">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="7ab33-162">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se používá tak, aby vyžadovala protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ab33-162">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="7ab33-163">`[RequireHttpsAttribute]` můžete uspořádání, řadiče nebo metod, nebo je možné uplatňovat globálně.</span><span class="sxs-lookup"><span data-stu-id="7ab33-163">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="7ab33-164">Použít atribut globálně, přidejte následující kód, který `ConfigureServices` v `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7ab33-164">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="7ab33-165">Předchozí zvýrazněný kód technologie používat všechny požadavky `HTTPS`, proto požadavky HTTP jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="7ab33-165">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="7ab33-166">Následující zvýrazněný kód přesměruje všechny požadavky HTTP na HTTPS:</span><span class="sxs-lookup"><span data-stu-id="7ab33-166">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="7ab33-167">Další informace najdete v tématu [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="7ab33-167">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="7ab33-168">Middleware také umožňuje, že aplikace nastaví stavový kód nebo kód stavu a port, který při spuštění přesměrování.</span><span class="sxs-lookup"><span data-stu-id="7ab33-168">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="7ab33-169">Globálně vyžadování protokolu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) je z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="7ab33-169">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="7ab33-170">Použití `[RequireHttps]` atribut na všechny řadiče/Razor Pages není považován za stejně bezpečné jako globálně vyžadování protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ab33-170">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="7ab33-171">Nebudete moct zaručit, `[RequireHttps]` atributu se použije při přidání nových řadičů a stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="7ab33-171">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="7ab33-172">Protokol zabezpečení striktní přenos HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="7ab33-172">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="7ab33-173">Za [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [striktní přenos HTTP zabezpečení (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) je vylepšení zabezpečení přihlášení, která je zadána pomocí hlavičky odpovědi webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7ab33-173">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="7ab33-174">Když [prohlížeč, který podporuje HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) obdrží toto záhlaví:</span><span class="sxs-lookup"><span data-stu-id="7ab33-174">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="7ab33-175">Prohlížeč ukládá konfiguraci pro doménu, která zabrání odeslání libovolné komunikaci přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="7ab33-175">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="7ab33-176">Prohlížeč vynutí veškerou komunikaci přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ab33-176">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="7ab33-177">Prohlížeč znemožní uživateli pomocí certifikátů nedůvěryhodný nebo neplatný.</span><span class="sxs-lookup"><span data-stu-id="7ab33-177">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="7ab33-178">Prohlížeč zakáže výzev, které umožní uživateli tohoto certifikátu dočasně důvěřovat.</span><span class="sxs-lookup"><span data-stu-id="7ab33-178">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="7ab33-179">Protože klient se vynucuje HSTS má určitá omezení:</span><span class="sxs-lookup"><span data-stu-id="7ab33-179">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="7ab33-180">Klient musí podporovat HSTS.</span><span class="sxs-lookup"><span data-stu-id="7ab33-180">The client must support HSTS.</span></span>
* <span data-ttu-id="7ab33-181">HSTS vyžaduje alespoň jeden úspěšného požadavku HTTPS k navázání HSTS zásad.</span><span class="sxs-lookup"><span data-stu-id="7ab33-181">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="7ab33-182">Aplikace musí zkontrolovat každého požadavku HTTP a přesměrování nebo zamítnutí žádosti protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="7ab33-182">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="7ab33-183">Implementuje HSTS s ASP.NET Core 2.1 nebo vyšší `UseHsts` – metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7ab33-183">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="7ab33-184">Následující kód volá `UseHsts` když aplikace není v [vývojový režim](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="7ab33-184">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="7ab33-185">`UseHsts` není doporučeno při vývoji vzhledem k tomu, že jsou nastavení HSTS dokonalé prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="7ab33-185">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="7ab33-186">Ve výchozím nastavení `UseHsts` nezahrnuje adresu místní zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="7ab33-186">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="7ab33-187">Pro produkční prostředí implementace HTTPS poprvé nastavte počáteční hodnotu HSTS malou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7ab33-187">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="7ab33-188">Nastavte hodnotu hodiny na no více než jeden den v případě, že budete potřebovat obnovit infrastruktury HTTPS do HTTP.</span><span class="sxs-lookup"><span data-stu-id="7ab33-188">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="7ab33-189">Poté, co jste si jisti udržitelnost konfigurace protokolu HTTPS, zvýšit hodnotu max-age HSTS. běžně používané hodnota je 1 rok.</span><span class="sxs-lookup"><span data-stu-id="7ab33-189">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="7ab33-190">Následující kód:</span><span class="sxs-lookup"><span data-stu-id="7ab33-190">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="7ab33-191">Nastaví parametr přednačtení záhlaví zabezpečení přenosu Strict.</span><span class="sxs-lookup"><span data-stu-id="7ab33-191">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="7ab33-192">Předinstalační není součástí [specifikaci RFC HSTS](https://tools.ietf.org/html/rfc6797), ale podporuje webových prohlížečů přednačtení HSTS weby na čerstvou instalaci.</span><span class="sxs-lookup"><span data-stu-id="7ab33-192">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="7ab33-193">Zobrazit [ https://hstspreload.org/ ](https://hstspreload.org/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="7ab33-193">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="7ab33-194">Umožňuje [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), které se vztahují zásady HSTS na hostiteli subdomény.</span><span class="sxs-lookup"><span data-stu-id="7ab33-194">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="7ab33-195">Explicitně nastaví parametr max-age záhlaví zabezpečení přenosu Strict na 60 dnů.</span><span class="sxs-lookup"><span data-stu-id="7ab33-195">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="7ab33-196">Pokud není nastavena výchozí hodnota je 30 dní.</span><span class="sxs-lookup"><span data-stu-id="7ab33-196">If not set, defaults to 30 days.</span></span> <span data-ttu-id="7ab33-197">Najdete v článku [max-age směrnice](https://tools.ietf.org/html/rfc6797#section-6.1.1) Další informace.</span><span class="sxs-lookup"><span data-stu-id="7ab33-197">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="7ab33-198">Přidá `example.com` do seznamu hostitelů mají vyloučit.</span><span class="sxs-lookup"><span data-stu-id="7ab33-198">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="7ab33-199">`UseHsts` vyloučí následující zpětné smyčky hostitele:</span><span class="sxs-lookup"><span data-stu-id="7ab33-199">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="7ab33-200">`localhost` : IPv4 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="7ab33-200">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="7ab33-201">`127.0.0.1` : IPv4 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="7ab33-201">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="7ab33-202">`[::1]` : IPv6 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="7ab33-202">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="7ab33-203">Předchozí příklad ukazuje, jak přidat další hostitele.</span><span class="sxs-lookup"><span data-stu-id="7ab33-203">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="7ab33-204">Výslovný nesouhlas s HTTPS při vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="7ab33-204">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="7ab33-205">Šablony ASP.NET Core 2.1 nebo novější webových aplikací (ze sady Visual Studio nebo příkazového řádku dotnet) povolit [HTTPS přesměrování](#require) a [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="7ab33-205">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="7ab33-206">Pro nasazení, které nevyžadují protokol HTTPS je můžete odhlásit protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ab33-206">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="7ab33-207">Například některé back-endových služeb, kde HTTPS se zpracovává externě na hraničních zařízeních, pomocí protokolu HTTPS v každém uzlu, není nutná.</span><span class="sxs-lookup"><span data-stu-id="7ab33-207">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node isn't needed.</span></span>

<span data-ttu-id="7ab33-208">Odhlásit protokolu HTTPS:</span><span class="sxs-lookup"><span data-stu-id="7ab33-208">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7ab33-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7ab33-209">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="7ab33-210">Zrušte zaškrtnutí políčka **konfigurace pro protokol HTTPS** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="7ab33-210">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Entity diagram](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7ab33-212">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="7ab33-212">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="7ab33-213">Použití `--no-https` možnost.</span><span class="sxs-lookup"><span data-stu-id="7ab33-213">Use the `--no-https` option.</span></span> <span data-ttu-id="7ab33-214">Příklad</span><span class="sxs-lookup"><span data-stu-id="7ab33-214">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="7ab33-215">Jak nastavit certifikát pro vývojáře pro Docker</span><span class="sxs-lookup"><span data-stu-id="7ab33-215">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="7ab33-216">Zobrazit [tento problém Githubu](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="7ab33-216">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="7ab33-217">Další informace</span><span class="sxs-lookup"><span data-stu-id="7ab33-217">Additional information</span></span>

* [<span data-ttu-id="7ab33-218">Podpora prohlížeče OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="7ab33-218">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
