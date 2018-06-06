---
title: Ukládání odpovědí do mezipaměti v ASP.NET Core
author: rick-anderson
description: Další informace o použití odpověď do mezipaměti pro nižší nároky na šířku pásma a zvýšit výkon aplikací ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.topic: article
uid: performance/caching/response
ms.openlocfilehash: e5a3877c68f8475e7dd49d44f4a92cf7b09ac7f5
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734507"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="e8d6e-103">Ukládání odpovědí do mezipaměti v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8d6e-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="e8d6e-104">Podle [Jan Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), a [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e8d6e-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="e8d6e-105">Ukládání odpovědí do mezipaměti na stránkách Razor je k dispozici v ASP.NET Core 2.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="e8d6e-106">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e8d6e-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e8d6e-107">Ukládání odpovědí do mezipaměti snižuje počet požadavků, které provede klient nebo server proxy webový server.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="e8d6e-108">Ukládání odpovědí do mezipaměti také snižuje množství práce provádí webového serveru pro generování odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="e8d6e-109">Ukládání odpovědí do mezipaměti řídí hlavičky, které určují, jakým způsobem chcete klienta, proxy server a middleware do mezipaměti odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="e8d6e-110">Webový server může ukládat do mezipaměti odpovědi při přidání [Middleware ukládání do mezipaměti odpovědi](xref:performance/caching/middleware).</span><span class="sxs-lookup"><span data-stu-id="e8d6e-110">The web server can cache responses when you add [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="e8d6e-111">Ukládání do mezipaměti založené na protokolu HTTP odpovědi</span><span class="sxs-lookup"><span data-stu-id="e8d6e-111">HTTP-based response caching</span></span>

<span data-ttu-id="e8d6e-112">[Ukládání do mezipaměti HTTP 1.1 specifikace](https://tools.ietf.org/html/rfc7234) popisuje chování mezipaměti Internetu.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-112">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="e8d6e-113">Primární záhlaví HTTP, který se používá pro ukládání do mezipaměti je [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), který slouží k určení mezipaměti *direktivy*.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-113">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="e8d6e-114">Direktivy řídí chování ukládání do mezipaměti jako požadavky zajistěte jejich představ od klientů na servery, a jako odpověď zajistěte jejich představ ze serverů zpět klientům.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-114">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="e8d6e-115">Požadavky a odpovědi pohyb proxy servery a proxy servery musí odpovídat také specifikace ukládání do mezipaměti HTTP 1.1.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-115">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="e8d6e-116">Běžné `Cache-Control` direktivy jsou uvedeny v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-116">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="e8d6e-117">– Direktiva</span><span class="sxs-lookup"><span data-stu-id="e8d6e-117">Directive</span></span>                                                       | <span data-ttu-id="e8d6e-118">Akce</span><span class="sxs-lookup"><span data-stu-id="e8d6e-118">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="e8d6e-119">public</span><span class="sxs-lookup"><span data-stu-id="e8d6e-119">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="e8d6e-120">Mezipaměť může ukládat odpověď.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-120">A cache may store the response.</span></span> |
| [<span data-ttu-id="e8d6e-121">private</span><span class="sxs-lookup"><span data-stu-id="e8d6e-121">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="e8d6e-122">Odpověď nesmí být uloženy ve sdílené mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-122">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="e8d6e-123">Privátní mezipaměti může uložit a opakovaně používat odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-123">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="e8d6e-124">Maximální stáří</span><span class="sxs-lookup"><span data-stu-id="e8d6e-124">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="e8d6e-125">Klient nebude přijímat odpovědi, jejichž stáří je větší než zadaný počet sekund.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-125">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="e8d6e-126">Příklady: `max-age=60` (60 sekund), `max-age=2592000` (1 měsíc)</span><span class="sxs-lookup"><span data-stu-id="e8d6e-126">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="e8d6e-127">no-cache</span><span class="sxs-lookup"><span data-stu-id="e8d6e-127">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="e8d6e-128">**U požadavků**: mezipaměti nesmí používat uložené odpovědi ke zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-128">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="e8d6e-129">Poznámka: Na zdrojový server znovu generuje odpovědi pro klienta a middleware aktualizuje odpověď uložená v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-129">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="e8d6e-130">**V odpovědi**: odpověď nesmí se používat pro následné žádosti bez ověřování na původním serveru.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-130">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="e8d6e-131">Ne – úložiště</span><span class="sxs-lookup"><span data-stu-id="e8d6e-131">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="e8d6e-132">**U požadavků**: žádost nesmí uložena mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-132">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="e8d6e-133">**V odpovědi**: mezipaměti nesmí ukládat libovolná součást odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-133">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="e8d6e-134">V následující tabulce jsou uvedeny další mezipaměti hlavičky, které hrají roli při ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-134">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="e8d6e-135">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="e8d6e-135">Header</span></span>                                                     | <span data-ttu-id="e8d6e-136">Funkce</span><span class="sxs-lookup"><span data-stu-id="e8d6e-136">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="e8d6e-137">stáří</span><span class="sxs-lookup"><span data-stu-id="e8d6e-137">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="e8d6e-138">Odhad množství času v sekundách, protože odpověď byla vygenerována nebo úspěšně ověřen na původním serveru.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-138">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="e8d6e-139">Vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="e8d6e-139">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="e8d6e-140">Datum a čas, po jejímž uplynutí je považován za odpověď zastaralých.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-140">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="e8d6e-141">Direktiva pragma</span><span class="sxs-lookup"><span data-stu-id="e8d6e-141">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="e8d6e-142">Pro zpětnou kompatibilitu s HTTP/1.0 ukládá do mezipaměti pro nastavení existuje `no-cache` chování.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-142">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="e8d6e-143">Pokud `Cache-Control` záhlaví nachází, `Pragma` záhlaví je ignorována.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-143">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="e8d6e-144">lišit</span><span class="sxs-lookup"><span data-stu-id="e8d6e-144">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="e8d6e-145">Určuje, že odpovědi v mezipaměti nesmí být odeslána, pokud všechny služby `Vary` záhlaví pole shodují v původní žádost odpověď uložená v mezipaměti a nový požadavek.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-145">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="e8d6e-146">Ukládání do mezipaměti ohledech založené na protokolu HTTP žádosti direktivy Cache-Control</span><span class="sxs-lookup"><span data-stu-id="e8d6e-146">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="e8d6e-147">[Ukládání do mezipaměti HTTP 1.1 specifikace pro hlavičku Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) vyžaduje mezipaměť pro respektovat platná `Cache-Control` hlavička odeslaná klientem.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-147">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="e8d6e-148">Klienta můžete provádět požadavky s `no-cache` hodnota hlavičky a force serveru pro generování novou odpověď pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-148">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="e8d6e-149">Vždy ctít zásady klienta `Cache-Control` hlavičky požadavku dává smysl, pokud uvažujete o cílem HTTP ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-149">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="e8d6e-150">Podle specifikace oficiální ukládání do mezipaměti slouží ke snížení režie latenci a sítě uspokojení požadavků v síti klientů, proxy servery a servery.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-150">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="e8d6e-151">Není to nutně způsob, jak řídit zatížení na původním serveru.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-151">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="e8d6e-152">Neexistuje žádný aktuální vývojáři řídit toto chování ukládání do mezipaměti při použití [Middleware ukládání do mezipaměti odpovědi](xref:performance/caching/middleware) protože middleware dodržuje oficiální specifikace ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-152">There's no current developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="e8d6e-153">[Budoucí vylepšení middleware](https://github.com/aspnet/ResponseCaching/issues/96) povolí přístup konfigurace middlewaru ignorovat požadavku `Cache-Control` záhlaví při rozhodování, která bude sloužit odpovědi v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-153">[Future enhancements to the middleware](https://github.com/aspnet/ResponseCaching/issues/96) will permit configuring the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="e8d6e-154">To nabídne možnost lepší řízení zatížení na serveru při použití middleware.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-154">This will offer you an opportunity to better control the load on your server when you use the middleware.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="e8d6e-155">Další ukládání do mezipaměti technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8d6e-155">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="e8d6e-156">Ukládání do mezipaměti v paměti</span><span class="sxs-lookup"><span data-stu-id="e8d6e-156">In-memory caching</span></span>

<span data-ttu-id="e8d6e-157">Ukládání do mezipaměti v paměti používá server paměti k ukládání data uložená v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-157">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="e8d6e-158">Tento typ ukládání do mezipaměti je vhodný pro jeden nebo více serverů pomocí *trvalé relace*.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-158">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="e8d6e-159">Trvalé relace znamená, že žádosti z klienta jsou vždy směrovány na stejný server pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-159">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="e8d6e-160">Další informace najdete v tématu [mezipaměti v paměti](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="e8d6e-160">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="e8d6e-161">Distribuované mezipaměti</span><span class="sxs-lookup"><span data-stu-id="e8d6e-161">Distributed Cache</span></span>

<span data-ttu-id="e8d6e-162">Distribuované mezipaměti využívat k ukládání dat v paměti, když aplikace hostovaná v cloudu nebo server farmy.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-162">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="e8d6e-163">Mezipaměť je sdílet mezi servery, které zpracovávají požadavky.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-163">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="e8d6e-164">Klient může odeslat svoji žádost, kterou provádí služba jakýkoli server ve skupině, pokud je k dispozici data uložená v mezipaměti klienta.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-164">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="e8d6e-165">ASP.NET Core nabízí systému SQL Server a Redis distribuované mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-165">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="e8d6e-166">Další informace najdete v tématu [pracovat s distribuované mezipaměti](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="e8d6e-166">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="e8d6e-167">Pomocník značky mezipaměti</span><span class="sxs-lookup"><span data-stu-id="e8d6e-167">Cache Tag Helper</span></span>

<span data-ttu-id="e8d6e-168">Pomocník značky mezipaměti můžete mezipaměti obsahu ze zobrazení MVC nebo stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-168">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="e8d6e-169">Pomocník značky mezipaměti používá k ukládání dat ukládání do mezipaměti v paměti.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-169">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="e8d6e-170">Další informace najdete v tématu [Pomocník značky mezipaměti ASP.NET MVC základní](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="e8d6e-170">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="e8d6e-171">Pomocník značky distribuované mezipaměti</span><span class="sxs-lookup"><span data-stu-id="e8d6e-171">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="e8d6e-172">Do mezipaměti můžete ukládat obsah ze zobrazení MVC nebo stránky Razor v distribuované cloudu nebo webové farmy scénáře Pomocník značky distribuované mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-172">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="e8d6e-173">Pomocník distribuované mezipaměti značku používá k ukládání dat systému SQL Server nebo Redis.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-173">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="e8d6e-174">Další informace najdete v tématu [distribuované mezipaměti značky pomocná](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="e8d6e-174">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="e8d6e-175">Atribut ResponseCache</span><span class="sxs-lookup"><span data-stu-id="e8d6e-175">ResponseCache attribute</span></span>

<span data-ttu-id="e8d6e-176">[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) Určuje parametry, které jsou nezbytné pro nastavení odpovídající hlavičky v ukládání odpovědí do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-176">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="e8d6e-177">Zakážete ukládání do mezipaměti pro obsah, který obsahuje informace pro klienty ověřené.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-177">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="e8d6e-178">Ukládání do mezipaměti by měla povoleno pouze pro obsah, který nemění na základě identity uživatele nebo jestli je uživatel přihlášený.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-178">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="e8d6e-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) uložené odpovědi se liší podle hodnoty daný seznam klíče dotazu.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="e8d6e-180">Když na jedinou hodnotu `*` je zadáno, se liší middleware odpovědí všechny žádosti o parametrů řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-180">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="e8d6e-181">`VaryByQueryKeys` Vyžaduje ASP.NET Core 1.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-181">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="e8d6e-182">Middleware ukládání do mezipaměti odpovědi musí být povoleno nastavení `VaryByQueryKeys` vlastnost; jinak, je vyvolána výjimka za běhu.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-182">The Response Caching Middleware must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="e8d6e-183">Není k dispozici odpovídající hlavičku HTTP pro `VaryByQueryKeys` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-183">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="e8d6e-184">Vlastnost je funkce protokolu HTTP zpracovávaných Middlewarem ukládání do mezipaměti odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-184">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="e8d6e-185">Pro middleware k obsluze odpovědi v mezipaměti řetězec dotazu a hodnotu řetězce dotazu musí odpovídat na předchozí požadavek.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-185">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="e8d6e-186">Představte si třeba pořadí požadavků a výsledky zobrazené v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-186">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="e8d6e-187">Požadavek</span><span class="sxs-lookup"><span data-stu-id="e8d6e-187">Request</span></span>                          | <span data-ttu-id="e8d6e-188">Výsledek</span><span class="sxs-lookup"><span data-stu-id="e8d6e-188">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="e8d6e-189">Vrácená serverem</span><span class="sxs-lookup"><span data-stu-id="e8d6e-189">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="e8d6e-190">Vrácená z middlewaru.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-190">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="e8d6e-191">Vrácená serverem</span><span class="sxs-lookup"><span data-stu-id="e8d6e-191">Returned from server</span></span>     |

<span data-ttu-id="e8d6e-192">První požadavek je vrácená serverem a uložené v mezipaměti v middlewaru.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-192">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="e8d6e-193">Druhá žádost se vrátí middlewarem, protože řetězec dotazu odpovídá předchozí požadavek.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-193">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="e8d6e-194">Třetí požadavek není v mezipaměti middleware, protože hodnotu řetězce dotazu se neshoduje se na předchozí požadavek.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-194">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="e8d6e-195">`ResponseCacheAttribute` Slouží ke konfiguraci a vytvoření (prostřednictvím `IFilterFactory`) [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span><span class="sxs-lookup"><span data-stu-id="e8d6e-195">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="e8d6e-196">`ResponseCacheFilter` Provede práci aktualizace příslušné hlavičky protokolu HTTP a funkce odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-196">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="e8d6e-197">Filtr:</span><span class="sxs-lookup"><span data-stu-id="e8d6e-197">The filter:</span></span>

* <span data-ttu-id="e8d6e-198">Odebere všechny existující hlavičky pro `Vary`, `Cache-Control`, a `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-198">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="e8d6e-199">Zapíše se příslušné hlavičky na základě vlastností nastavit `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-199">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="e8d6e-200">Aktualizace odpověď do mezipaměti funkce protokolu HTTP, pokud `VaryByQueryKeys` nastavena.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-200">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="e8d6e-201">lišit</span><span class="sxs-lookup"><span data-stu-id="e8d6e-201">Vary</span></span>

<span data-ttu-id="e8d6e-202">Tuto hlavičku je zapsat, pouze když `VaryByHeader` je nastavena.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-202">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="e8d6e-203">Je nastaven na hodnotu `Vary` hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-203">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="e8d6e-204">Následující ukázkové používá `VaryByHeader` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="e8d6e-204">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8d6e-205">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="e8d6e-205">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e8d6e-206">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="e8d6e-206">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]</span></span>

::: moniker-end

<span data-ttu-id="e8d6e-207">Můžete zobrazit hlavičky odpovědi pomocí nástrojů v prohlížeči na síti.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-207">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="e8d6e-208">Následující obrázek ukazuje F12 Edge výstup na **sítě** při `About2` se aktualizují metodu akce:</span><span class="sxs-lookup"><span data-stu-id="e8d6e-208">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![Při volání metody akce About2 okraj F12 výstup na kartě sítě](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="e8d6e-210">NoStore a Location.None</span><span class="sxs-lookup"><span data-stu-id="e8d6e-210">NoStore and Location.None</span></span>

<span data-ttu-id="e8d6e-211">`NoStore` přepíše většina jiných vlastností.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-211">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="e8d6e-212">Pokud je tato vlastnost nastavená na `true`, `Cache-Control` záhlaví je nastaven na `no-store`.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-212">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="e8d6e-213">Pokud `Location` je nastaven na `None`:</span><span class="sxs-lookup"><span data-stu-id="e8d6e-213">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="e8d6e-214">`Cache-Control` je nastavena na `no-store,no-cache`.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-214">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="e8d6e-215">`Pragma` je nastavena na `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-215">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="e8d6e-216">Pokud `NoStore` je `false` a `Location` je `None`, `Cache-Control` a `Pragma` jsou nastaveny na `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-216">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="e8d6e-217">Obvykle nastavíte `NoStore` k `true` na chybové stránky.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-217">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="e8d6e-218">Příklad:</span><span class="sxs-lookup"><span data-stu-id="e8d6e-218">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8d6e-219">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="e8d6e-219">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e8d6e-220">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="e8d6e-220">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]</span></span>

::: moniker-end

<span data-ttu-id="e8d6e-221">Výsledkem je následující hlavičky:</span><span class="sxs-lookup"><span data-stu-id="e8d6e-221">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="e8d6e-222">Umístění a doba trvání</span><span class="sxs-lookup"><span data-stu-id="e8d6e-222">Location and Duration</span></span>

<span data-ttu-id="e8d6e-223">Chcete-li povolit ukládání do mezipaměti, `Duration` musí být nastavena na kladnou hodnotu a `Location` musí být buď `Any` (výchozí) nebo `Client`.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-223">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="e8d6e-224">V takovém případě `Cache-Control` záhlaví je nastaven na hodnotu umístění, za nímž následuje `max-age` odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-224">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="e8d6e-225">`Location`na možnosti `Any` a `Client` převede na `Cache-Control` hodnoty hlavičky `public` a `private`, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-225">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="e8d6e-226">Jak je uvedeno dříve, nastavení `Location` k `None` nastaví obě `Cache-Control` a `Pragma` záhlaví `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-226">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="e8d6e-227">Níže je příkladem zobrazujícím hlavičky produkovaný nastavení `Duration` a ponechat výchozí `Location` hodnotu:</span><span class="sxs-lookup"><span data-stu-id="e8d6e-227">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8d6e-228">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="e8d6e-228">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e8d6e-229">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="e8d6e-229">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]</span></span>

::: moniker-end

<span data-ttu-id="e8d6e-230">Tímto se vytvoří následující hlavičky:</span><span class="sxs-lookup"><span data-stu-id="e8d6e-230">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="e8d6e-231">Profily mezipaměti</span><span class="sxs-lookup"><span data-stu-id="e8d6e-231">Cache profiles</span></span>

<span data-ttu-id="e8d6e-232">Místo duplikování `ResponseCache` nastavení na mnoha atributů akce řadič, mezipaměť profily se dají konfigurovat jako možnosti při nastavování MVC v `ConfigureServices` metoda v `Startup`.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-232">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="e8d6e-233">Hodnoty zjištěné v profilu odkazované mezipaměti jsou použity jako výchozí pomocí `ResponseCache` atribut a jsou přepsaná podle libovolných vlastností zadaná pro atribut.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-233">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="e8d6e-234">Nastavení profilu mezipaměti:</span><span class="sxs-lookup"><span data-stu-id="e8d6e-234">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8d6e-235">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="e8d6e-235">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e8d6e-236">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="e8d6e-236">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

<span data-ttu-id="e8d6e-237">Odkazování na profil mezipaměti:</span><span class="sxs-lookup"><span data-stu-id="e8d6e-237">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8d6e-238">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]</span><span class="sxs-lookup"><span data-stu-id="e8d6e-238">[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e8d6e-239">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]</span><span class="sxs-lookup"><span data-stu-id="e8d6e-239">[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]</span></span>

::: moniker-end

<span data-ttu-id="e8d6e-240">`ResponseCache` Atribut lze použít jak pro akce (metody) a řadiče (třídy).</span><span class="sxs-lookup"><span data-stu-id="e8d6e-240">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="e8d6e-241">Atributy na úrovni metody přepsat nastavení zadané v atributy na úrovni třídy.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-241">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="e8d6e-242">V předchozím příkladu atribut úrovni třídy určuje dobu trvání 30 sekund, zatímco metoda úrovni atribut odkazuje profil mezipaměti s určitou dobou trvání nastavena na 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="e8d6e-242">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="e8d6e-243">Výsledný hlavičky:</span><span class="sxs-lookup"><span data-stu-id="e8d6e-243">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="e8d6e-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e8d6e-244">Additional resources</span></span>

* [<span data-ttu-id="e8d6e-245">Ukládání odpovědí do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="e8d6e-245">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="e8d6e-246">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="e8d6e-246">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [<span data-ttu-id="e8d6e-247">Mezipaměť v paměti</span><span class="sxs-lookup"><span data-stu-id="e8d6e-247">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="e8d6e-248">Práce s distribuovanou mezipamětí</span><span class="sxs-lookup"><span data-stu-id="e8d6e-248">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="e8d6e-249">Detekovat změny s tokeny změn</span><span class="sxs-lookup"><span data-stu-id="e8d6e-249">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="e8d6e-250">Middleware pro ukládání odpovědí do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="e8d6e-250">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="e8d6e-251">Uložení pomocné rutiny značky do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="e8d6e-251">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="e8d6e-252">Pomocná rutina značek v distribuované mezipaměti</span><span class="sxs-lookup"><span data-stu-id="e8d6e-252">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
