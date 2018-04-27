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
ms.openlocfilehash: 0509bebe430c6ba213031a2cb7cb91bb7a39566d
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="32063-103">Vynutit HTTPS v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="32063-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="32063-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="32063-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="32063-105">Tento dokument ukazuje, jak:</span><span class="sxs-lookup"><span data-stu-id="32063-105">This document shows how to:</span></span>

- <span data-ttu-id="32063-106">Vyžadovat protokol HTTPS pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="32063-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="32063-107">Přesměrujte všechny požadavky HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="32063-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="32063-108">Proveďte **není** použít `RequireHttpsAttribute` na webovým rozhraním API, která zobrazit citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="32063-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="32063-109">`RequireHttpsAttribute` stavové kódy HTTP se používá k přesměrování prohlížeče z HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="32063-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="32063-110">Rozhraní API klienti nemusí pochopit a orientují přesměrování z HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="32063-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="32063-111">Tito klienti mohou odesílat informace přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="32063-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="32063-112">Webová rozhraní API se musí buď:</span><span class="sxs-lookup"><span data-stu-id="32063-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="32063-113">Nelze naslouchat na protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="32063-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="32063-114">Ukončení připojení se stavovým kódem 400 (Chybný požadavek) a není požadavek vyřídit.</span><span class="sxs-lookup"><span data-stu-id="32063-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="32063-115">Vyžadovat protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="32063-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="32063-117">Doporučujeme všechny ASP.NET Core volání webové aplikace `UseHttpsRedirection` přesměrovat všechny požadavky HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="32063-117">We recommend all ASP.NET Core web apps call `UseHttpsRedirection` to redirect all HTTP requests to HTTPS.</span></span> <span data-ttu-id="32063-118">Pokud `UseHsts` je volána v aplikaci, musí být volána před provedením `UseHttpsRedirection`.</span><span class="sxs-lookup"><span data-stu-id="32063-118">If `UseHsts` is called in the app, it must be called before `UseHttpsRedirection`.</span></span>

<span data-ttu-id="32063-119">Následující kód volání `UseHttpsRedirection` v `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="32063-119">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="32063-120">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="32063-120">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>


<span data-ttu-id="32063-121">Následující kód:</span><span class="sxs-lookup"><span data-stu-id="32063-121">The following code:</span></span>

<span data-ttu-id="32063-122">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="32063-122">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

* <span data-ttu-id="32063-123">Nastaví `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="32063-123">Sets `RedirectStatusCode`.</span></span>
* <span data-ttu-id="32063-124">Nastaví HTTPS port 5001.</span><span class="sxs-lookup"><span data-stu-id="32063-124">Sets the HTTPS port to 5001.</span></span>

::: moniker-end


::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="32063-125">[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) se používá k vyžadují protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="32063-125">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="32063-126">`[RequireHttpsAttribute]` můžete uspořádání řadiče nebo metod, nebo mohou být použity globálně.</span><span class="sxs-lookup"><span data-stu-id="32063-126">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="32063-127">Použít atribut globálně, přidejte následující kód do `ConfigureServices` v `Startup`:</span><span class="sxs-lookup"><span data-stu-id="32063-127">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="32063-128">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="32063-128">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="32063-129">Předchozí zvýrazněný vyžaduje všechny požadavky používat `HTTPS`; proto požadavky HTTP jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="32063-129">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="32063-130">Následující zvýrazněný kód přesměruje všech požadavků HTTP do HTTPS:</span><span class="sxs-lookup"><span data-stu-id="32063-130">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="32063-131">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="32063-131">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="32063-132">Další informace najdete v tématu [URL přepisování Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="32063-132">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="32063-133">Globálně vyžadujících protokol HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) je nejlepším postupem zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="32063-133">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="32063-134">Použití `[RequireHttps]` atribut na všechny řadiče nebo Razor stránky se nepovažuje za bezpečnou jako jako globální vyžadujících protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="32063-134">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="32063-135">Nebudete moct zaručit `[RequireHttps]` atribut se používá, když se přidají nové řadiče a stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="32063-135">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="32063-136">Protokol zabezpečení striktní přenos HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="32063-136">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="32063-137">Za [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [striktní přenos HTTP zabezpečení (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) je určený parametrem webové aplikace prostřednictvím hlavičky odpovědi speciální vylepšení zabezpečení opt-in.</span><span class="sxs-lookup"><span data-stu-id="32063-137">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="32063-138">Jakmile podporovaného prohlížeče obdrží tuto hlavičku prohlížeč tohoto zabrání veškerou komunikaci odesílání prostřednictvím protokolu HTTP k zadané doméně a místo toho odešle veškerou komunikaci přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="32063-138">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="32063-139">Rovněž zamezí klikněte na protokol HTTPS přes výzvy v prohlížečích.</span><span class="sxs-lookup"><span data-stu-id="32063-139">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="32063-140">Technologie ASP.NET pro základní 2.1 preview1 nebo později implementuje HSTS s `UseHsts` metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="32063-140">ASP.NET Core 2.1 preview1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="32063-141">Následující kód volání `UseHsts` Pokud aplikace není v [režimu pro vývoj](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="32063-141">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="32063-142">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="32063-142">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="32063-143">`UseHsts` není doporučujeme při vývoji protože záhlaví HSTS je vysoce cachable pomocí prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="32063-143">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="32063-144">Ve výchozím nastavení vyloučí UseHsts adresu místní smyčky.</span><span class="sxs-lookup"><span data-stu-id="32063-144">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="32063-145">Následující kód:</span><span class="sxs-lookup"><span data-stu-id="32063-145">The following code:</span></span>

<span data-ttu-id="32063-146">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="32063-146">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="32063-147">Nastaví parametr přednačtení záhlaví zabezpečení přenosu Strict.</span><span class="sxs-lookup"><span data-stu-id="32063-147">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="32063-148">Předběžného načítání není součástí [specifikaci RFC HSTS](https://tools.ietf.org/html/rfc6797), ale podporuje webových prohlížečů přednačtení HSTS weby na čerstvou instalaci.</span><span class="sxs-lookup"><span data-stu-id="32063-148">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="32063-149">V tématu [ https://hstspreload.org/ ](https://hstspreload.org/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="32063-149">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="32063-150">Umožňuje [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), které se vztahují zásady HSTS na hostiteli subdomény.</span><span class="sxs-lookup"><span data-stu-id="32063-150">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="32063-151">Explicitně nastaví parametr maximální stáří zabezpečení přenosu Strict hlavičky k na 60 dnů.</span><span class="sxs-lookup"><span data-stu-id="32063-151">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="32063-152">Pokud není nastavena, výchozí hodnota je 30 dní.</span><span class="sxs-lookup"><span data-stu-id="32063-152">If not set, defaults to 30 days.</span></span> <span data-ttu-id="32063-153">Najdete v článku [maximální stáří direktiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) Další informace.</span><span class="sxs-lookup"><span data-stu-id="32063-153">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="32063-154">Přidá `example.com` do seznamu hostitelů, které chcete vyloučit.</span><span class="sxs-lookup"><span data-stu-id="32063-154">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="32063-155">`UseHsts` vyloučí následující zpětné smyčky hostitele:</span><span class="sxs-lookup"><span data-stu-id="32063-155">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="32063-156">`localhost` : IPv4 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="32063-156">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="32063-157">`127.0.0.1` : IPv4 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="32063-157">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="32063-158">`[::1]` : IPv6 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="32063-158">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="32063-159">Předchozí příklad ukazuje, jak přidat další hostitele.</span><span class="sxs-lookup"><span data-stu-id="32063-159">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="32063-160">Výslovný nesouhlas s HTTPS na vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="32063-160">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="32063-161">ASP.NET Core 2.1 a novější šablony webové aplikace (ze sady Visual Studio nebo pomocí příkazového řádku dotnet) umožňují [HTTPS přesměrování](#require) a [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="32063-161">The ASP.NET Core 2.1 and later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="32063-162">Pro nasazení, které nevyžadují protokol HTTPS které můžete výslovný nesouhlas s protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="32063-162">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="32063-163">Například není potřeba některé služby back-end, kde HTTPS je zpracovanou externě na okraji a pomocí protokolu HTTPS v každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="32063-163">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="32063-164">Vyjádřit výslovný nesouhlas protokolu HTTPS:</span><span class="sxs-lookup"><span data-stu-id="32063-164">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="32063-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32063-165">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="32063-166">Zrušte zaškrtnutí políčka **konfigurace pro protokol HTTPS** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="32063-166">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Entity diagram](enforcing-ssl/_static/out.png)


#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="32063-168">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="32063-168">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="32063-169">Použití `--no-https` možnost.</span><span class="sxs-lookup"><span data-stu-id="32063-169">Use the `--no-https` option.</span></span> <span data-ttu-id="32063-170">Příklad</span><span class="sxs-lookup"><span data-stu-id="32063-170">For example</span></span>

```cli
dotnet new razor --no-https
```

------

::: moniker-end