---
title: Ukládání odpovědí do mezipaměti v ASP.NET Core
author: rick-anderson
description: Další informace o použití odpověď do mezipaměti pro nižší požadavky na šířku pásma a zvýšit výkon aplikace ASP.NET Core.
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: bbf5b649bac9d31aa6d0ecdc3828648677b05716
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090690"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="91b7e-103">Ukládání odpovědí do mezipaměti v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="91b7e-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="91b7e-104">Podle [Jan Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), a [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="91b7e-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="91b7e-105">Ukládání odpovědí do mezipaměti v Razor Pages je k dispozici v ASP.NET Core 2.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="91b7e-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="91b7e-106">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="91b7e-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="91b7e-107">Ukládání odpovědí do mezipaměti snižuje počet požadavků, které odešle klient nebo server proxy webový server.</span><span class="sxs-lookup"><span data-stu-id="91b7e-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="91b7e-108">Ukládání odpovědí do mezipaměti také snižuje množství práce provádí webového serveru pro generování odpovědi.</span><span class="sxs-lookup"><span data-stu-id="91b7e-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="91b7e-109">Ukládání odpovědí do mezipaměti se řídí hlavičky, které určují, jak chcete klienta, serveru proxy a middlewarem do mezipaměti odpovědi.</span><span class="sxs-lookup"><span data-stu-id="91b7e-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="91b7e-110">Webový server může ukládat do mezipaměti odpovědi při přidání [Middleware pro ukládání do mezipaměti odpovědí](xref:performance/caching/middleware).</span><span class="sxs-lookup"><span data-stu-id="91b7e-110">The web server can cache responses when you add [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="91b7e-111">Ukládání do mezipaměti založené na protokolu HTTP odpovědi</span><span class="sxs-lookup"><span data-stu-id="91b7e-111">HTTP-based response caching</span></span>

<span data-ttu-id="91b7e-112">[Specifikace HTTP 1.1 ukládání do mezipaměti](https://tools.ietf.org/html/rfc7234) popisuje chování mezipaměti Internet.</span><span class="sxs-lookup"><span data-stu-id="91b7e-112">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="91b7e-113">Primární záhlaví HTTP používá pro ukládání do mezipaměti je [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), který se používá k určení mezipaměti *direktivy*.</span><span class="sxs-lookup"><span data-stu-id="91b7e-113">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="91b7e-114">Direktivy řízení chování ukládání do mezipaměti podle požadavků dostanou od klientů na servery a jako odpověď dostanou ze serverů zpět klientům.</span><span class="sxs-lookup"><span data-stu-id="91b7e-114">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="91b7e-115">Požadavky a odpovědi procházet proxy servery a proxy servery musí také odpovídají specifikaci HTTP 1.1 ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-115">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="91b7e-116">Běžné `Cache-Control` direktivy jsou uvedeny v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="91b7e-116">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="91b7e-117">– Direktiva</span><span class="sxs-lookup"><span data-stu-id="91b7e-117">Directive</span></span>                                                       | <span data-ttu-id="91b7e-118">Akce</span><span class="sxs-lookup"><span data-stu-id="91b7e-118">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="91b7e-119">public</span><span class="sxs-lookup"><span data-stu-id="91b7e-119">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="91b7e-120">Mezipaměť může ukládat odpovědi.</span><span class="sxs-lookup"><span data-stu-id="91b7e-120">A cache may store the response.</span></span> |
| [<span data-ttu-id="91b7e-121">private</span><span class="sxs-lookup"><span data-stu-id="91b7e-121">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="91b7e-122">Odpověď nesmí být uloženy ve sdílené mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-122">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="91b7e-123">Soukromé mezipaměti může ukládat a opakovaně používat odpovědi.</span><span class="sxs-lookup"><span data-stu-id="91b7e-123">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="91b7e-124">Maximální stáří</span><span class="sxs-lookup"><span data-stu-id="91b7e-124">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="91b7e-125">Klient nebude přijímat odpovědi, jejichž stáří je větší než zadaný počet sekund.</span><span class="sxs-lookup"><span data-stu-id="91b7e-125">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="91b7e-126">Příklady: `max-age=60` (60 sekund), `max-age=2592000` (1 měsíc)</span><span class="sxs-lookup"><span data-stu-id="91b7e-126">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="91b7e-127">no-cache</span><span class="sxs-lookup"><span data-stu-id="91b7e-127">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="91b7e-128">**U požadavků**: mezipaměť nesmí používat uložené odpovědi, abyste vyhověli žádosti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-128">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="91b7e-129">Poznámka: Odpověď na zdrojový server znovu vygeneruje pro klienta a middleware aktualizuje odpověď na uložené v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-129">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="91b7e-130">**V odpovědi**: odpověď nesmí se používat pro další požadavek bez ověřování na původním serveru.</span><span class="sxs-lookup"><span data-stu-id="91b7e-130">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="91b7e-131">no-store</span><span class="sxs-lookup"><span data-stu-id="91b7e-131">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="91b7e-132">**U požadavků**: mezipaměť nesmí uložit žádost.</span><span class="sxs-lookup"><span data-stu-id="91b7e-132">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="91b7e-133">**V odpovědi**: mezipaměť nesmí uložit libovolnou část odpovědi.</span><span class="sxs-lookup"><span data-stu-id="91b7e-133">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="91b7e-134">V následující tabulce jsou uvedeny další hlavičky mezipaměti, které hrají roli při ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-134">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="91b7e-135">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="91b7e-135">Header</span></span>                                                     | <span data-ttu-id="91b7e-136">Funkce</span><span class="sxs-lookup"><span data-stu-id="91b7e-136">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="91b7e-137">Stáří</span><span class="sxs-lookup"><span data-stu-id="91b7e-137">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="91b7e-138">Odhad množství času v sekundách, protože odpověď byla vygenerována nebo úspěšně ověřen na původním serveru.</span><span class="sxs-lookup"><span data-stu-id="91b7e-138">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="91b7e-139">Vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="91b7e-139">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="91b7e-140">Datum a čas, po jejímž uplynutí se považuje za odpověď zastaralá.</span><span class="sxs-lookup"><span data-stu-id="91b7e-140">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="91b7e-141">Direktiva pragma</span><span class="sxs-lookup"><span data-stu-id="91b7e-141">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="91b7e-142">Pro zpětnou kompatibilitu s HTTP verze 1.0 ukládá do mezipaměti pro nastavení existuje `no-cache` chování.</span><span class="sxs-lookup"><span data-stu-id="91b7e-142">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="91b7e-143">Pokud `Cache-Control` záhlaví je k dispozici, `Pragma` záhlaví se ignoruje.</span><span class="sxs-lookup"><span data-stu-id="91b7e-143">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="91b7e-144">se liší</span><span class="sxs-lookup"><span data-stu-id="91b7e-144">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="91b7e-145">Určuje, že odpověď uložená v mezipaměti nesmí být odeslána, pokud všechny nástroje `Vary` záhlaví pole shodují v původní požadavek odpověď uložená v mezipaměti a nový požadavek.</span><span class="sxs-lookup"><span data-stu-id="91b7e-145">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="91b7e-146">Ukládání do mezipaměti respektuje založené na protokolu HTTP požadavku direktivy Cache-Control</span><span class="sxs-lookup"><span data-stu-id="91b7e-146">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="91b7e-147">[Specifikace HTTP 1.1 ukládání do mezipaměti pro hlavičku Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) vyžaduje mezipaměti případném dalším sdílení dodržovat platné `Cache-Control` záhlaví odesílaném klientem.</span><span class="sxs-lookup"><span data-stu-id="91b7e-147">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="91b7e-148">Klienta můžete vytvářet požadavky s `no-cache` hodnota hlavičky a vynutit server vygenerovat novou odpověď pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="91b7e-148">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="91b7e-149">Vždy dodržením klienta `Cache-Control` hlavičky žádosti dává smysl, pokud považujete za cíl protokolu HTTP, ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-149">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="91b7e-150">V části specifikaci oficiální ukládání do mezipaměti smyslem je snížit latenci a sítě režii splňující požadavky napříč sítí klientů, proxy servery a servery.</span><span class="sxs-lookup"><span data-stu-id="91b7e-150">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="91b7e-151">Není nutně způsob, jak řídit zatížení na původním serveru.</span><span class="sxs-lookup"><span data-stu-id="91b7e-151">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="91b7e-152">Neexistuje žádné aktuální vývojáři řídit tohoto chování ukládání do mezipaměti při použití [Middleware pro ukládání do mezipaměti odpovědí](xref:performance/caching/middleware) vzhledem k tomu, že dodržuje middleware official je přínosné pro ukládání do mezipaměti specifikace.</span><span class="sxs-lookup"><span data-stu-id="91b7e-152">There's no current developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="91b7e-153">[Budoucí vylepšení pro middleware](https://github.com/aspnet/ResponseCaching/issues/96) umožní konfigurace middlewaru, který má požadavek ignorovat `Cache-Control` hlavičku při rozhodování o tom, která bude sloužit odpověď uložená v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-153">[Future enhancements to the middleware](https://github.com/aspnet/ResponseCaching/issues/96) will permit configuring the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="91b7e-154">To vám nabídne možnost získat lepší kontrolu nad zatížení na serveru při použití middlewaru.</span><span class="sxs-lookup"><span data-stu-id="91b7e-154">This will offer you an opportunity to better control the load on your server when you use the middleware.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="91b7e-155">Další technologie ukládání do mezipaměti v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="91b7e-155">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="91b7e-156">Ukládání do mezipaměti v paměti</span><span class="sxs-lookup"><span data-stu-id="91b7e-156">In-memory caching</span></span>

<span data-ttu-id="91b7e-157">Ukládání do mezipaměti v paměti používá k ukládání dat uložených v mezipaměti paměti serveru.</span><span class="sxs-lookup"><span data-stu-id="91b7e-157">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="91b7e-158">Tento typ ukládání do mezipaměti je vhodný pro jeden nebo více servery pomocí *rychlé relace*.</span><span class="sxs-lookup"><span data-stu-id="91b7e-158">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="91b7e-159">Rychlé relace znamená, že požadavky od klienta jsou vždy směrovány na stejný server ke zpracování.</span><span class="sxs-lookup"><span data-stu-id="91b7e-159">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="91b7e-160">Další informace najdete v tématu [ukládat do mezipaměti v paměti](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="91b7e-160">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="91b7e-161">Distribuované mezipaměti</span><span class="sxs-lookup"><span data-stu-id="91b7e-161">Distributed Cache</span></span>

<span data-ttu-id="91b7e-162">K ukládání dat v paměti, když je aplikace hostovaná v cloudu nebo serveru farmy pomocí distribuované mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-162">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="91b7e-163">Mezipaměť je sdílen mezi servery, které zpracovávají požadavky.</span><span class="sxs-lookup"><span data-stu-id="91b7e-163">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="91b7e-164">Klient může odeslat žádost, kterou provádí služba jakýkoli server ve skupině, pokud je k dispozici data uložená v mezipaměti klienta.</span><span class="sxs-lookup"><span data-stu-id="91b7e-164">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="91b7e-165">ASP.NET Core nabízí systému SQL Server a mezipamětí Redis distribuovat.</span><span class="sxs-lookup"><span data-stu-id="91b7e-165">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="91b7e-166">Další informace naleznete v tématu <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="91b7e-166">For more information, see <xref:performance/caching/distributed>.</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="91b7e-167">Pomocné rutiny značky do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="91b7e-167">Cache Tag Helper</span></span>

<span data-ttu-id="91b7e-168">Vám může ukládat do mezipaměti obsah ze zobrazení MVC nebo stránky Razor s pomocné rutiny značky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-168">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="91b7e-169">Pomocná rutina značek mezipaměti používá k ukládání dat do mezipaměti v paměti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-169">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="91b7e-170">Další informace najdete v tématu [pomocná rutina značek v mezipaměti v ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="91b7e-170">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="91b7e-171">Pomocná rutina značek v distribuované mezipaměti</span><span class="sxs-lookup"><span data-stu-id="91b7e-171">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="91b7e-172">Obsah ze zobrazení MVC nebo stránky Razor v distribuovaných cloudových nebo webových farem můžete mezipaměti pomocí distribuované pomocná rutina značek mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-172">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="91b7e-173">Distribuované mezipaměti pomocná rutina značek v používá k ukládání dat serveru SQL Server nebo Redis.</span><span class="sxs-lookup"><span data-stu-id="91b7e-173">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="91b7e-174">Další informace najdete v tématu [pomocné rutiny značky distribuované mezipaměti](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="91b7e-174">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="91b7e-175">Atribut ResponseCache</span><span class="sxs-lookup"><span data-stu-id="91b7e-175">ResponseCache attribute</span></span>

<span data-ttu-id="91b7e-176">[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) Určuje parametry, které jsou nezbytné pro nastavení příslušné záhlaví v ukládání odpovědí do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-176">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="91b7e-177">Zakáže ukládání do mezipaměti pro obsah, který obsahuje informace o ověření klienti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-177">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="91b7e-178">Ukládání do mezipaměti musí být povolené pouze pro obsah, který se nezmění na základě identity uživatele nebo určuje, zda je uživatel přihlášený.</span><span class="sxs-lookup"><span data-stu-id="91b7e-178">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="91b7e-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) uložené odpovědi se liší podle hodnoty daný seznam klíče dotazu.</span><span class="sxs-lookup"><span data-stu-id="91b7e-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="91b7e-180">Když na jedinou hodnotu `*` je k dispozici, se liší middleware odpovědi na všechny žádosti parametrů řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="91b7e-180">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="91b7e-181">`VaryByQueryKeys` vyžaduje ASP.NET Core 1.1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="91b7e-181">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="91b7e-182">Middleware pro ukládání do mezipaměti odpovědí musí být povoleno nastavení `VaryByQueryKeys` vlastnosti; v opačném případě je vyvolána výjimka za běhu.</span><span class="sxs-lookup"><span data-stu-id="91b7e-182">The Response Caching Middleware must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="91b7e-183">Není k dispozici odpovídající hlavičku protokolu HTTP `VaryByQueryKeys` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="91b7e-183">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="91b7e-184">Vlastnost je funkce protokolu HTTP zpracovávaných middlewarem odpovědi ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-184">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="91b7e-185">Pro daný middleware pro obsluhu odpověď uložená v mezipaměti řetězec dotazu a hodnotu řetězce dotazu musí odpovídat předchozí žádosti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-185">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="91b7e-186">Představte si třeba pořadí požadavků a výsledky zobrazené v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="91b7e-186">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="91b7e-187">Požadavek</span><span class="sxs-lookup"><span data-stu-id="91b7e-187">Request</span></span>                          | <span data-ttu-id="91b7e-188">Výsledek</span><span class="sxs-lookup"><span data-stu-id="91b7e-188">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="91b7e-189">Server vrátil</span><span class="sxs-lookup"><span data-stu-id="91b7e-189">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="91b7e-190">Vrácená z middlewaru</span><span class="sxs-lookup"><span data-stu-id="91b7e-190">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="91b7e-191">Server vrátil</span><span class="sxs-lookup"><span data-stu-id="91b7e-191">Returned from server</span></span>     |

<span data-ttu-id="91b7e-192">První požadavek je vrácená serverem a uložili do mezipaměti v middlewaru.</span><span class="sxs-lookup"><span data-stu-id="91b7e-192">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="91b7e-193">Druhý požadavek je vrátit middlewarem, protože řetězec dotazu odpovídá předchozí žádosti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-193">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="91b7e-194">Třetí žádost není v mezipaměti middleware, protože hodnotu řetězce dotazu neodpovídá předchozí žádosti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-194">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="91b7e-195">`ResponseCacheAttribute` Slouží ke konfiguraci a vytvořte (prostřednictvím `IFilterFactory`) [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span><span class="sxs-lookup"><span data-stu-id="91b7e-195">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="91b7e-196">`ResponseCacheFilter` Provádí aktualizace správné hlavičky HTTP a funkce odpovědi.</span><span class="sxs-lookup"><span data-stu-id="91b7e-196">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="91b7e-197">Filtr:</span><span class="sxs-lookup"><span data-stu-id="91b7e-197">The filter:</span></span>

* <span data-ttu-id="91b7e-198">Odebere všechny existující záhlaví pro `Vary`, `Cache-Control`, a `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="91b7e-198">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="91b7e-199">Zapíše příslušné záhlaví podle vlastnosti nastavené `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="91b7e-199">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="91b7e-200">Aktualizace ukládání do mezipaměti funkce protokolu HTTP, pokud odpověď `VaryByQueryKeys` nastavena.</span><span class="sxs-lookup"><span data-stu-id="91b7e-200">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="91b7e-201">se liší</span><span class="sxs-lookup"><span data-stu-id="91b7e-201">Vary</span></span>

<span data-ttu-id="91b7e-202">Této hlavičky je zapsán, pouze když `VaryByHeader` je nastavena.</span><span class="sxs-lookup"><span data-stu-id="91b7e-202">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="91b7e-203">Je nastaven na hodnotu `Vary` hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="91b7e-203">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="91b7e-204">Následující ukázkový používá `VaryByHeader` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="91b7e-204">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="91b7e-205">Můžete zobrazit pomocí nástrojů v prohlížeči sítě hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="91b7e-205">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="91b7e-206">Následující obrázek ukazuje F12 Edge výstupních **sítě** kartu, kdy `About2` aktualizaci metody akce:</span><span class="sxs-lookup"><span data-stu-id="91b7e-206">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![Při volání metody akce About2 hraniční F12 výstup na síťové kartě](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="91b7e-208">NoStore a Location.None</span><span class="sxs-lookup"><span data-stu-id="91b7e-208">NoStore and Location.None</span></span>

<span data-ttu-id="91b7e-209">`NoStore` přepíše většinu dalších vlastností.</span><span class="sxs-lookup"><span data-stu-id="91b7e-209">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="91b7e-210">Pokud je tato vlastnost nastavena na `true`, `Cache-Control` záhlaví je nastavena na `no-store`.</span><span class="sxs-lookup"><span data-stu-id="91b7e-210">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="91b7e-211">Pokud `Location` je nastavena na `None`:</span><span class="sxs-lookup"><span data-stu-id="91b7e-211">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="91b7e-212">`Cache-Control` je nastavena na `no-store,no-cache`.</span><span class="sxs-lookup"><span data-stu-id="91b7e-212">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="91b7e-213">`Pragma` je nastavena na `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="91b7e-213">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="91b7e-214">Pokud `NoStore` je `false` a `Location` je `None`, `Cache-Control` a `Pragma` jsou nastaveny na `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="91b7e-214">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="91b7e-215">Obvykle nastavena `NoStore` k `true` na chybové stránky.</span><span class="sxs-lookup"><span data-stu-id="91b7e-215">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="91b7e-216">Příklad:</span><span class="sxs-lookup"><span data-stu-id="91b7e-216">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="91b7e-217">Výsledkem je následující hlavičky:</span><span class="sxs-lookup"><span data-stu-id="91b7e-217">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="91b7e-218">Umístění a doba trvání</span><span class="sxs-lookup"><span data-stu-id="91b7e-218">Location and Duration</span></span>

<span data-ttu-id="91b7e-219">Chcete-li povolit ukládání do mezipaměti, `Duration` musí být nastaven na kladnou hodnotu a `Location` musí být buď `Any` (výchozí) nebo `Client`.</span><span class="sxs-lookup"><span data-stu-id="91b7e-219">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="91b7e-220">V takovém případě `Cache-Control` záhlaví je nastavena na hodnotu umístění, za nímž následuje `max-age` odpovědi.</span><span class="sxs-lookup"><span data-stu-id="91b7e-220">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="91b7e-221">`Location`v možnosti `Any` a `Client` překlad do `Cache-Control` hodnoty hlavičky `public` a `private`v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="91b7e-221">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="91b7e-222">Jak bylo uvedeno dříve, nastavení `Location` k `None` nastaví obě `Cache-Control` a `Pragma` záhlaví `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="91b7e-222">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="91b7e-223">Níže je příklad ukazující hlavičky vytvořen tak, že nastavíte `Duration` a výchozí `Location` hodnotu:</span><span class="sxs-lookup"><span data-stu-id="91b7e-223">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="91b7e-224">Tímto se vytvoří následující hlavičky:</span><span class="sxs-lookup"><span data-stu-id="91b7e-224">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="91b7e-225">Profily mezipaměti</span><span class="sxs-lookup"><span data-stu-id="91b7e-225">Cache profiles</span></span>

<span data-ttu-id="91b7e-226">Namísto duplikování `ResponseCache` nastavení na mnoho atributů akce kontroleru, mezipaměti profily se dají konfigurovat jako možnosti při nastavování MVC v `ConfigureServices` metoda ve `Startup`.</span><span class="sxs-lookup"><span data-stu-id="91b7e-226">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="91b7e-227">Hodnoty nalezené v profilu odkazované mezipaměti se používají jako výchozí hodnoty podle `ResponseCache` atribut a jsou přepsány podle libovolných vlastností zadané v atributu.</span><span class="sxs-lookup"><span data-stu-id="91b7e-227">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="91b7e-228">Nastavení profilu mezipaměti:</span><span class="sxs-lookup"><span data-stu-id="91b7e-228">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="91b7e-229">Odkazování na profil mezipaměti:</span><span class="sxs-lookup"><span data-stu-id="91b7e-229">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="91b7e-230">`ResponseCache` Atribut se dá použít k akce (metody) a kontrolerů (třídy).</span><span class="sxs-lookup"><span data-stu-id="91b7e-230">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="91b7e-231">Atributy na úrovni metody přepsání nastavení uvedená v atributy na úrovni třídy.</span><span class="sxs-lookup"><span data-stu-id="91b7e-231">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="91b7e-232">V příkladu výše Určuje atribut úroveň třídy v délce 30 sekund, přestože úroveň metody atribut odkazuje profil mezipaměti s určitou dobou trvání nastaven na 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="91b7e-232">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="91b7e-233">Výsledný záhlaví:</span><span class="sxs-lookup"><span data-stu-id="91b7e-233">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="91b7e-234">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="91b7e-234">Additional resources</span></span>

* [<span data-ttu-id="91b7e-235">Ukládání odpovědí do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="91b7e-235">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="91b7e-236">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="91b7e-236">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
