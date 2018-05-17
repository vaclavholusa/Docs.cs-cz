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
ms.openlocfilehash: edc69443455677ba80ebb0a73e193d4d6741e470
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="17828-103">Vynutit HTTPS v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="17828-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="17828-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="17828-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="17828-105">Tento dokument ukazuje, jak:</span><span class="sxs-lookup"><span data-stu-id="17828-105">This document shows how to:</span></span>

- <span data-ttu-id="17828-106">Vyžadovat protokol HTTPS pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="17828-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="17828-107">Přesměrujte všechny požadavky HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="17828-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="17828-108">Proveďte **není** použít `RequireHttpsAttribute` na webovým rozhraním API, která zobrazit citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="17828-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="17828-109">`RequireHttpsAttribute` stavové kódy HTTP se používá k přesměrování prohlížeče z HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="17828-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="17828-110">Rozhraní API klienti nemusí pochopit a orientují přesměrování z HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="17828-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="17828-111">Tito klienti mohou odesílat informace přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="17828-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="17828-112">Webová rozhraní API se musí buď:</span><span class="sxs-lookup"><span data-stu-id="17828-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="17828-113">Nelze naslouchat na protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="17828-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="17828-114">Ukončení připojení se stavovým kódem 400 (Chybný požadavek) a není požadavek vyřídit.</span><span class="sxs-lookup"><span data-stu-id="17828-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="17828-115">Vyžadovat protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="17828-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="17828-116">Doporučujeme všechny ASP.NET Core volání webové aplikace `UseHttpsRedirection` přesměrovat všechny požadavky HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="17828-116">We recommend all ASP.NET Core web apps call `UseHttpsRedirection` to redirect all HTTP requests to HTTPS.</span></span> <span data-ttu-id="17828-117">Pokud `UseHsts` je volána v aplikaci, musí být volána před provedením `UseHttpsRedirection`.</span><span class="sxs-lookup"><span data-stu-id="17828-117">If `UseHsts` is called in the app, it must be called before `UseHttpsRedirection`.</span></span>

<span data-ttu-id="17828-118">Následující kód volání `UseHttpsRedirection` v `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="17828-118">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="17828-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="17828-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>


<span data-ttu-id="17828-120">Následující kód:</span><span class="sxs-lookup"><span data-stu-id="17828-120">The following code:</span></span>

<span data-ttu-id="17828-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="17828-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

* <span data-ttu-id="17828-122">Nastaví `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="17828-122">Sets `RedirectStatusCode`.</span></span>
* <span data-ttu-id="17828-123">Nastaví HTTPS port 5001.</span><span class="sxs-lookup"><span data-stu-id="17828-123">Sets the HTTPS port to 5001.</span></span>

::: moniker-end


::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="17828-124">[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) se používá k vyžadují protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="17828-124">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="17828-125">`[RequireHttpsAttribute]` můžete uspořádání řadiče nebo metod, nebo mohou být použity globálně.</span><span class="sxs-lookup"><span data-stu-id="17828-125">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="17828-126">Použít atribut globálně, přidejte následující kód do `ConfigureServices` v `Startup`:</span><span class="sxs-lookup"><span data-stu-id="17828-126">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="17828-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="17828-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="17828-128">Předchozí zvýrazněný vyžaduje všechny požadavky používat `HTTPS`; proto požadavky HTTP jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="17828-128">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="17828-129">Následující zvýrazněný kód přesměruje všech požadavků HTTP do HTTPS:</span><span class="sxs-lookup"><span data-stu-id="17828-129">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="17828-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="17828-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="17828-131">Další informace najdete v tématu [URL přepisování Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="17828-131">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="17828-132">Globálně vyžadujících protokol HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) je nejlepším postupem zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="17828-132">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="17828-133">Použití `[RequireHttps]` atribut na všechny řadiče nebo Razor stránky se nepovažuje za bezpečnou jako jako globální vyžadujících protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="17828-133">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="17828-134">Nebudete moct zaručit `[RequireHttps]` atribut se používá, když se přidají nové řadiče a stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="17828-134">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="17828-135">Protokol zabezpečení striktní přenos HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="17828-135">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="17828-136">Za [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [striktní přenos HTTP zabezpečení (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) je určený parametrem webové aplikace prostřednictvím hlavičky odpovědi speciální vylepšení zabezpečení opt-in.</span><span class="sxs-lookup"><span data-stu-id="17828-136">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="17828-137">Jakmile podporovaného prohlížeče obdrží tuto hlavičku prohlížeč tohoto zabrání veškerou komunikaci odesílání prostřednictvím protokolu HTTP k zadané doméně a místo toho odešle veškerou komunikaci přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="17828-137">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="17828-138">Rovněž zamezí klikněte na protokol HTTPS přes výzvy v prohlížečích.</span><span class="sxs-lookup"><span data-stu-id="17828-138">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="17828-139">Implementuje HSTS s ASP.NET Core 2.1 nebo vyšší `UseHsts` metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="17828-139">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="17828-140">Následující kód volání `UseHsts` Pokud aplikace není v [režimu pro vývoj](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="17828-140">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="17828-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="17828-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="17828-142">`UseHsts` není doporučujeme při vývoji protože záhlaví HSTS je vysoce cachable pomocí prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="17828-142">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="17828-143">Ve výchozím nastavení vyloučí UseHsts adresu místní smyčky.</span><span class="sxs-lookup"><span data-stu-id="17828-143">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="17828-144">Následující kód:</span><span class="sxs-lookup"><span data-stu-id="17828-144">The following code:</span></span>

<span data-ttu-id="17828-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="17828-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="17828-146">Nastaví parametr přednačtení záhlaví zabezpečení přenosu Strict.</span><span class="sxs-lookup"><span data-stu-id="17828-146">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="17828-147">Předběžného načítání není součástí [specifikaci RFC HSTS](https://tools.ietf.org/html/rfc6797), ale podporuje webových prohlížečů přednačtení HSTS weby na čerstvou instalaci.</span><span class="sxs-lookup"><span data-stu-id="17828-147">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="17828-148">V tématu [ https://hstspreload.org/ ](https://hstspreload.org/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="17828-148">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="17828-149">Umožňuje [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), které se vztahují zásady HSTS na hostiteli subdomény.</span><span class="sxs-lookup"><span data-stu-id="17828-149">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="17828-150">Explicitně nastaví parametr maximální stáří zabezpečení přenosu Strict hlavičky k na 60 dnů.</span><span class="sxs-lookup"><span data-stu-id="17828-150">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="17828-151">Pokud není nastavena, výchozí hodnota je 30 dní.</span><span class="sxs-lookup"><span data-stu-id="17828-151">If not set, defaults to 30 days.</span></span> <span data-ttu-id="17828-152">Najdete v článku [maximální stáří direktiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) Další informace.</span><span class="sxs-lookup"><span data-stu-id="17828-152">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="17828-153">Přidá `example.com` do seznamu hostitelů, které chcete vyloučit.</span><span class="sxs-lookup"><span data-stu-id="17828-153">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="17828-154">`UseHsts` vyloučí následující zpětné smyčky hostitele:</span><span class="sxs-lookup"><span data-stu-id="17828-154">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="17828-155">`localhost` : IPv4 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="17828-155">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="17828-156">`127.0.0.1` : IPv4 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="17828-156">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="17828-157">`[::1]` : IPv6 adresu zpětné smyčky.</span><span class="sxs-lookup"><span data-stu-id="17828-157">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="17828-158">Předchozí příklad ukazuje, jak přidat další hostitele.</span><span class="sxs-lookup"><span data-stu-id="17828-158">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="17828-159">Výslovný nesouhlas s HTTPS na vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="17828-159">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="17828-160">ASP.NET Core 2.1 a novější šablony webové aplikace (ze sady Visual Studio nebo pomocí příkazového řádku dotnet) umožňují [HTTPS přesměrování](#require) a [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="17828-160">The ASP.NET Core 2.1 and later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="17828-161">Pro nasazení, které nevyžadují protokol HTTPS které můžete výslovný nesouhlas s protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="17828-161">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="17828-162">Například není potřeba některé služby back-end, kde HTTPS je zpracovanou externě na okraji a pomocí protokolu HTTPS v každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="17828-162">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="17828-163">Vyjádřit výslovný nesouhlas protokolu HTTPS:</span><span class="sxs-lookup"><span data-stu-id="17828-163">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="17828-164">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="17828-164">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="17828-165">Zrušte zaškrtnutí políčka **konfigurace pro protokol HTTPS** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="17828-165">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Entity diagram](enforcing-ssl/_static/out.png)


#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="17828-167">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="17828-167">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="17828-168">Použití `--no-https` možnost.</span><span class="sxs-lookup"><span data-stu-id="17828-168">Use the `--no-https` option.</span></span> <span data-ttu-id="17828-169">Příklad</span><span class="sxs-lookup"><span data-stu-id="17828-169">For example</span></span>

```cli
dotnet new razor --no-https
```

------

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="17828-170">Postup instalace certifikátu vývojáře pro Docker</span><span class="sxs-lookup"><span data-stu-id="17828-170">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="17828-171">V tématu [potíže Githubu](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="17828-171">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
