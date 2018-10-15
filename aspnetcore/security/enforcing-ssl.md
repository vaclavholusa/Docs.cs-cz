---
title: Vynucení protokolu HTTPS v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak chcete vyžadovat protokol HTTPS/TLS ve webové aplikaci ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/enforcing-ssl
ms.openlocfilehash: b4c058d3b4f00276043d9520d756e62ed8cac5d9
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325598"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="6cbb1-103">Vynucení protokolu HTTPS v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6cbb1-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="6cbb1-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6cbb1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6cbb1-105">Tento dokument ukazuje, jak:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-105">This document shows how to:</span></span>

* <span data-ttu-id="6cbb1-106">Vyžadovat protokol HTTPS pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="6cbb1-107">Přesměrujte všechny požadavky HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="6cbb1-108">Žádné rozhraní API můžete zabránit posláním citlivých dat na první požadavek klienta.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="6cbb1-109">Proveďte **není** použít [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) na webová rozhraní API, která zobrazit citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="6cbb1-110">`RequireHttpsAttribute` stavové kódy HTTP se používá pro přesměrování prohlížeče z HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="6cbb1-111">Klienti rozhraní API nemusí pochopit a dodržují přesměrování z HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="6cbb1-112">Tito klienti mohou odesílat informace přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="6cbb1-113">Webová rozhraní API by buď:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="6cbb1-114">Nelze naslouchat na HTTP.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="6cbb1-115">Ukončete připojení s stavový kód 400 (Chybný požadavek) a není obsluhovat žádosti.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>

## <a name="require-https"></a><span data-ttu-id="6cbb1-116">Vyžadovat protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="6cbb1-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6cbb1-117">Doporučujeme všechny produkční ASP.NET Core volání webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="6cbb1-118">Middleware přesměrování protokolu HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) přesměrovat všechny požadavky HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="6cbb1-119">[UseHsts](#hsts), přenosový striktní bezpečnostní protokol HTTP (HSTS).</span><span class="sxs-lookup"><span data-stu-id="6cbb1-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="6cbb1-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="6cbb1-120">UseHttpsRedirection</span></span>

<span data-ttu-id="6cbb1-121">Následující kód volá `UseHttpsRedirection` v `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="6cbb1-122">Předchozí zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="6cbb1-123">Používá výchozí [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="6cbb1-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="6cbb1-124">Používá výchozí [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), pokud není přepsán `ASPNETCORE_HTTPS_PORT` proměnné prostředí nebo [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="6cbb1-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="6cbb1-125">Doporučujeme používat dočasné přesměrování spíše než trvalé přesměrování jako odkaz ukládání do mezipaměti může způsobit nestabilní chování ve vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-125">We recommend using temporary redirects rather than permanent redirects, as link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="6cbb1-126">Doporučujeme používat [HSTS](#hsts) na signál pro klienty, kteří pouze zabezpečit prostředek požadavků odesílaných do aplikace (jenom v produkčním prostředí).</span><span class="sxs-lookup"><span data-stu-id="6cbb1-126">We recommend using [HSTS](#hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

> [!WARNING]
> <span data-ttu-id="6cbb1-127">Port musí být k dispozici pro daný middleware přesměrovat na protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-127">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="6cbb1-128">Pokud je k dispozici žádný port, nedojde k přesměrování na protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-128">If no port is available, redirection to HTTPS doesn't occur.</span></span> <span data-ttu-id="6cbb1-129">HTTPS port se dá nastavit některým z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-129">The HTTPS port can be specified using any of the following approaches:</span></span>
>
> * <span data-ttu-id="6cbb1-130">Nastavte `HttpsRedirectionOptions.HttpsPort`.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-130">Set `HttpsRedirectionOptions.HttpsPort`.</span></span>
> * <span data-ttu-id="6cbb1-131">Nastavte `ASPNETCORE_HTTPS_PORT` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-131">Set the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
> * <span data-ttu-id="6cbb1-132">Při vývoji, nastavte adresu URL HTTPS *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-132">In development, set an HTTPS URL in *launchsettings.json*.</span></span>
> * <span data-ttu-id="6cbb1-133">Konfigurace koncového bodu adresy URL HTTPS [Kestrel](xref:fundamentals/servers/kestrel) nebo [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="6cbb1-133">Configure an HTTPS URL endpoint for [Kestrel](xref:fundamentals/servers/kestrel) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>
>
> <span data-ttu-id="6cbb1-134">Při Kestrel nebo ovladač HTTP.sys se používá jako veřejnou hraniční server, musí být Kestrel nebo ovladač HTTP.sys nakonfigurovaná k naslouchání na obojí:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-134">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>
>
> * <span data-ttu-id="6cbb1-135">Zabezpečený port, ve kterém se klient přesměruje (obvykle 443 v produkčním prostředí a 5001 ve vývoji).</span><span class="sxs-lookup"><span data-stu-id="6cbb1-135">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
> * <span data-ttu-id="6cbb1-136">Nezabezpečené port (standardně 80 v produkčním prostředí) a 5000 ve vývoji.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-136">The insecure port (typically, 80 in production and 5000 in development).</span></span>
>
> <span data-ttu-id="6cbb1-137">Nezabezpečené port musí být přístupné pro klienta v pořadí pro aplikaci pro příjem nezabezpečený požadavek a přesměrování na zabezpečený port.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-137">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect it to the secure port.</span></span>
>
> <span data-ttu-id="6cbb1-138">Všechny brány firewall mezi klientem a serverem musí mít také porty otevřené pro provoz.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-138">Any firewall between the client and server must also have the ports open for traffic.</span></span>
>
> <span data-ttu-id="6cbb1-139">Další informace najdete v tématu [konfigurace koncového bodu Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) nebo <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-139">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

<span data-ttu-id="6cbb1-140">Následující zvýrazněný kód volá [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) konfigurace middlewaru možností:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-140">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="6cbb1-141">Volání `AddHttpsRedirection` jenom je potřeba změnit hodnoty `HttpsPort` nebo `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-141">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="6cbb1-142">Předchozí zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-142">The preceding highlighted code:</span></span>

* <span data-ttu-id="6cbb1-143">Nastaví [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) k [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), což je výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-143">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), which is the default value.</span></span> <span data-ttu-id="6cbb1-144">Použijte pole [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) třídu pro přiřazení k `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-144">Use the fields of the [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="6cbb1-145">Nastaví HTTPS port 5001.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-145">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="6cbb1-146">Výchozí hodnota je 443.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-146">The default value is 443.</span></span>

<span data-ttu-id="6cbb1-147">Automaticky nastavit port následujících mechanismů:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-147">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="6cbb1-148">Middleware najdou porty prostřednictvím [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) když platí tyto podmínky:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-148">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

  * <span data-ttu-id="6cbb1-149">Kestrel nebo ovladač HTTP.sys se používá přímo s koncovým bodům protokol HTTPS (platí také pro spuštění aplikace pomocí ladicího programu nástroje Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="6cbb1-149">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  * <span data-ttu-id="6cbb1-150">Pouze **jeden port HTTPS** používají aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-150">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="6cbb1-151">Visual Studio se používá:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-151">Visual Studio is used:</span></span>

  * <span data-ttu-id="6cbb1-152">Služba IIS Express má povolený protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-152">IIS Express has HTTPS enabled.</span></span>
  * <span data-ttu-id="6cbb1-153">*launchSettings.json* nastaví `sslPort` pro službu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-153">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="6cbb1-154">Při spuštění aplikace za reverzní proxy server (například služby IIS, IIS Express), `IServerAddressesFeature` není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-154">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="6cbb1-155">Port musí být ručně nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-155">The port must be manually configured.</span></span> <span data-ttu-id="6cbb1-156">Pokud port není nastavena, přesměrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-156">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="6cbb1-157">Port, který se dá nakonfigurovat pomocí nastavení [nastavení konfigurace webového hostitele https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="6cbb1-157">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="6cbb1-158">**Klíč**: https_port</span><span class="sxs-lookup"><span data-stu-id="6cbb1-158">**Key**: https_port</span></span>  
<span data-ttu-id="6cbb1-159">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="6cbb1-159">**Type**: *string*</span></span>  
<span data-ttu-id="6cbb1-160">**Výchozí**: výchozí hodnota není nastavená.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-160">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="6cbb1-161">**Sada s použitím**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="6cbb1-161">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="6cbb1-162">**Proměnná prostředí**: `<PREFIX_>HTTPS_PORT` (předpona je `ASPNETCORE_` při použití webového hostitele.)</span><span class="sxs-lookup"><span data-stu-id="6cbb1-162">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="6cbb1-163">Port, který se dá nakonfigurovat nepřímo pomocí nastavení adresy URL s `ASPNETCORE_URLS` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-163">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="6cbb1-164">Proměnná prostředí nakonfiguruje server a pak middleware nepřímo zjistí port HTTPS přes `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-164">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="6cbb1-165">Pokud je nastaven žádný port:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-165">If no port is set:</span></span>

* <span data-ttu-id="6cbb1-166">Přesměrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-166">Requests aren't redirected.</span></span>
* <span data-ttu-id="6cbb1-167">Middleware protokoluje upozornění "Nepovedlo se určit port https pro přesměrování."</span><span class="sxs-lookup"><span data-stu-id="6cbb1-167">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="6cbb1-168">Alternativou k používání HTTPS přesměrování Middleware (`UseHttpsRedirection`), je použít Middleware pro přepis adres URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="6cbb1-168">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="6cbb1-169">`AddRedirectToHttps` Můžete také nastavit stavový kód a portu při spuštění přesměrování.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-169">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="6cbb1-170">Další informace najdete v tématu [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="6cbb1-170">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="6cbb1-171">Při přesměrování na HTTPS bez nutnosti další přesměrování pravidla, doporučujeme používat Middleware přesměrování protokolu HTTPS (`UseHttpsRedirection`) popsané v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-171">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="6cbb1-172">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se používá tak, aby vyžadovala protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-172">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="6cbb1-173">`[RequireHttpsAttribute]` můžete uspořádání, řadiče nebo metod, nebo je možné uplatňovat globálně.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-173">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="6cbb1-174">Použít atribut globálně, přidejte následující kód, který `ConfigureServices` v `Startup`:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-174">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="6cbb1-175">Předchozí zvýrazněný kód technologie používat všechny požadavky `HTTPS`, proto požadavky HTTP jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-175">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="6cbb1-176">Následující zvýrazněný kód přesměruje všechny požadavky HTTP na HTTPS:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-176">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="6cbb1-177">Další informace najdete v tématu [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="6cbb1-177">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="6cbb1-178">Middleware také umožňuje, že aplikace nastaví stavový kód nebo kód stavu a port, který při spuštění přesměrování.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-178">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="6cbb1-179">Globálně vyžadování protokolu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) je z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-179">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="6cbb1-180">Použití `[RequireHttps]` atribut na všechny řadiče/Razor Pages není považován za stejně bezpečné jako globálně vyžadování protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-180">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="6cbb1-181">Nebudete moct zaručit, `[RequireHttps]` atributu se použije při přidání nových řadičů a stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-181">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="6cbb1-182">Protokol zabezpečení striktní přenos HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="6cbb1-182">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="6cbb1-183">Za [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [striktní přenos HTTP zabezpečení (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) je vylepšení zabezpečení přihlášení, která je zadána pomocí hlavičky odpovědi webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-183">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="6cbb1-184">Když [prohlížeč, který podporuje HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) obdrží toto záhlaví:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-184">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="6cbb1-185">Prohlížeč ukládá konfiguraci pro doménu, která zabrání odeslání libovolné komunikaci přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-185">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="6cbb1-186">Prohlížeč vynutí veškerou komunikaci přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-186">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="6cbb1-187">Prohlížeč znemožní uživateli pomocí certifikátů nedůvěryhodný nebo neplatný.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-187">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="6cbb1-188">Prohlížeč zakáže výzev, které umožní uživateli tohoto certifikátu dočasně důvěřovat.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-188">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="6cbb1-189">Protože klient se vynucuje HSTS má určitá omezení:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-189">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="6cbb1-190">Klient musí podporovat HSTS.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-190">The client must support HSTS.</span></span>
* <span data-ttu-id="6cbb1-191">HSTS vyžaduje alespoň jeden úspěšného požadavku HTTPS k navázání HSTS zásad.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-191">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="6cbb1-192">Aplikace musí zkontrolovat každého požadavku HTTP a přesměrování nebo zamítnutí žádosti protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-192">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="6cbb1-193">Implementuje HSTS s ASP.NET Core 2.1 nebo vyšší `UseHsts` – metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-193">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="6cbb1-194">Následující kód volá `UseHsts` když aplikace není v [vývojový režim](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="6cbb1-194">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="6cbb1-195">`UseHsts` není doporučeno při vývoji vzhledem k tomu, že jsou nastavení HSTS dokonalé prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-195">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="6cbb1-196">Ve výchozím nastavení `UseHsts` nezahrnuje adresu místní zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-196">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="6cbb1-197">Pro produkční prostředí implementace HTTPS poprvé nastavte počáteční hodnotu HSTS malou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-197">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="6cbb1-198">Nastavte hodnotu hodiny na no více než jeden den v případě, že budete potřebovat obnovit infrastruktury HTTPS do HTTP.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-198">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="6cbb1-199">Poté, co jste si jisti udržitelnost konfigurace protokolu HTTPS, zvýšit hodnotu max-age HSTS. běžně používané hodnota je 1 rok.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-199">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="6cbb1-200">Následující kód:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-200">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="6cbb1-201">Nastaví parametr přednačtení záhlaví zabezpečení přenosu Strict.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-201">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="6cbb1-202">Předinstalační není součástí [specifikaci RFC HSTS](https://tools.ietf.org/html/rfc6797), ale podporuje webových prohlížečů přednačtení HSTS weby na čerstvou instalaci.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-202">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="6cbb1-203">Zobrazit [ https://hstspreload.org/ ](https://hstspreload.org/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-203">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="6cbb1-204">Umožňuje [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), které se vztahují zásady HSTS na hostiteli subdomény.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-204">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="6cbb1-205">Explicitně nastaví parametr max-age záhlaví zabezpečení přenosu Strict na 60 dnů.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-205">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="6cbb1-206">Pokud není nastavena výchozí hodnota je 30 dní.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-206">If not set, defaults to 30 days.</span></span> <span data-ttu-id="6cbb1-207">Najdete v článku [max-age směrnice](https://tools.ietf.org/html/rfc6797#section-6.1.1) Další informace.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-207">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="6cbb1-208">Přidá `example.com` do seznamu hostitelů mají vyloučit.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-208">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="6cbb1-209">`UseHsts` vyloučí následující zpětné smyčky hostitele:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-209">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="6cbb1-210">`localhost` : IPv4 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-210">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="6cbb1-211">`127.0.0.1` : IPv4 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-211">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="6cbb1-212">`[::1]` : IPv6 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-212">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="6cbb1-213">Předchozí příklad ukazuje, jak přidat další hostitele.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-213">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="6cbb1-214">Odhlásit HTTPS/HSTS při vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="6cbb1-214">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="6cbb1-215">V některých scénářích služby back-endu kde zabezpečení připojení probíhá v veřejnou hraniční části sítě není vyžadována konfigurace zabezpečení připojení v každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-215">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="6cbb1-216">Webová aplikace vygenerované ze šablony v sadě Visual Studio nebo z [dotnet nové](/dotnet/core/tools/dotnet-new) povolit příkaz [HTTPS přesměrování](#require) a [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="6cbb1-216">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="6cbb1-217">Pro nasazení, které nevyžadují tyto scénáře které můžete výslovný nesouhlas s HTTPS/HSTS při vytvoření aplikace ze šablony.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-217">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="6cbb1-218">Chcete odhlásit HTTPS/HSTS:</span><span class="sxs-lookup"><span data-stu-id="6cbb1-218">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cbb1-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cbb1-219">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="6cbb1-220">Zrušte zaškrtnutí políčka **konfigurace pro protokol HTTPS** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-220">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Entity diagram](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6cbb1-222">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="6cbb1-222">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="6cbb1-223">Použití `--no-https` možnost.</span><span class="sxs-lookup"><span data-stu-id="6cbb1-223">Use the `--no-https` option.</span></span> <span data-ttu-id="6cbb1-224">Příklad</span><span class="sxs-lookup"><span data-stu-id="6cbb1-224">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="6cbb1-225">Jak nastavit certifikát pro vývojáře pro Docker</span><span class="sxs-lookup"><span data-stu-id="6cbb1-225">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="6cbb1-226">Zobrazit [tento problém Githubu](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="6cbb1-226">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="6cbb1-227">Další informace</span><span class="sxs-lookup"><span data-stu-id="6cbb1-227">Additional information</span></span>

* [<span data-ttu-id="6cbb1-228">Podpora prohlížeče OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="6cbb1-228">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
