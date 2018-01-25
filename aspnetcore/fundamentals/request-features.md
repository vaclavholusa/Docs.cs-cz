---
title: "Žádost o funkce ASP.NET Core"
author: ardalis
description: "Další informace o webového serveru implementace podrobnosti týkající se požadavků HTTP a odpovědí, které jsou definovány v rozhraní pro ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: f0e371f5ea6c6688ef32adcacf667a412e4625e5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="request-features-in-aspnet-core"></a><span data-ttu-id="3b34d-103">Žádost o funkce ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3b34d-103">Request Features in ASP.NET Core</span></span>

<span data-ttu-id="3b34d-104">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="3b34d-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="3b34d-105">Související podrobnosti implementace webového serveru na požadavky HTTP a odpovědí jsou definovány v rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3b34d-105">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="3b34d-106">Tato rozhraní jsou používány implementací serveru a middleware, vytvářet a upravovat hostování kanálu aplikace.</span><span class="sxs-lookup"><span data-stu-id="3b34d-106">These interfaces are used by server implementations and middleware to create and modify the application's hosting pipeline.</span></span>

## <a name="feature-interfaces"></a><span data-ttu-id="3b34d-107">Funkce rozhraní</span><span class="sxs-lookup"><span data-stu-id="3b34d-107">Feature interfaces</span></span>

<span data-ttu-id="3b34d-108">Definuje počet HTTP funkce rozhraní ASP.NET Core `Microsoft.AspNetCore.Http.Features` servery které se používají k identifikaci funkce podporují.</span><span class="sxs-lookup"><span data-stu-id="3b34d-108">ASP.NET Core defines a number of HTTP feature interfaces in `Microsoft.AspNetCore.Http.Features` which are used by servers to identify the features they support.</span></span> <span data-ttu-id="3b34d-109">Následující funkce rozhraní zpracování požadavků a odpovědí vrátit:</span><span class="sxs-lookup"><span data-stu-id="3b34d-109">The following feature interfaces handle requests and return responses:</span></span>

<span data-ttu-id="3b34d-110">`IHttpRequestFeature`Definuje strukturu požadavku HTTP, včetně protokolu, cesta, řetězec dotazu, hlavičky a text.</span><span class="sxs-lookup"><span data-stu-id="3b34d-110">`IHttpRequestFeature` Defines the structure of an HTTP request, including the protocol, path, query string, headers, and body.</span></span>

<span data-ttu-id="3b34d-111">`IHttpResponseFeature`Definuje strukturu odpovědi HTTP, včetně stavový kód, hlavičky a text odpovědi.</span><span class="sxs-lookup"><span data-stu-id="3b34d-111">`IHttpResponseFeature` Defines the structure of an HTTP response, including the status code, headers, and body of the response.</span></span>

<span data-ttu-id="3b34d-112">`IHttpAuthenticationFeature`Definuje podpory identifikace uživatele na základě `ClaimsPrincipal` a zadání obslužnou rutinu ověřování.</span><span class="sxs-lookup"><span data-stu-id="3b34d-112">`IHttpAuthenticationFeature` Defines support for identifying users based on a `ClaimsPrincipal` and specifying an authentication handler.</span></span>

<span data-ttu-id="3b34d-113">`IHttpUpgradeFeature`Definuje podporu pro [HTTP upgrady](https://tools.ietf.org/html/rfc2616.html#section-14.42), které umožňují klienta k určení, které další protokoly, ho chcete použít, pokud server, které chcete přepnout protokoly.</span><span class="sxs-lookup"><span data-stu-id="3b34d-113">`IHttpUpgradeFeature` Defines support for [HTTP Upgrades](https://tools.ietf.org/html/rfc2616.html#section-14.42), which allow the client to specify which additional protocols it would like to use if the server wishes to switch protocols.</span></span>

<span data-ttu-id="3b34d-114">`IHttpBufferingFeature`Definuje metody pro zakázání ukládání do vyrovnávací paměti požadavků a odpovědí.</span><span class="sxs-lookup"><span data-stu-id="3b34d-114">`IHttpBufferingFeature` Defines methods for disabling buffering of requests and/or responses.</span></span>

<span data-ttu-id="3b34d-115">`IHttpConnectionFeature`Definuje vlastnosti pro místní a vzdálené adresy a porty.</span><span class="sxs-lookup"><span data-stu-id="3b34d-115">`IHttpConnectionFeature` Defines properties for local and remote addresses and ports.</span></span>

<span data-ttu-id="3b34d-116">`IHttpRequestLifetimeFeature`Definuje podporu pro přerušení připojení nebo zjišťování, pokud požadavek byl ukončen předčasně, například jako službou na odpojení klienta.</span><span class="sxs-lookup"><span data-stu-id="3b34d-116">`IHttpRequestLifetimeFeature` Defines support for aborting connections, or detecting if a request has been terminated prematurely, such as by a client disconnect.</span></span>

<span data-ttu-id="3b34d-117">`IHttpSendFileFeature`Definuje metody pro asynchronní odesílání souborů.</span><span class="sxs-lookup"><span data-stu-id="3b34d-117">`IHttpSendFileFeature` Defines a method for sending files asynchronously.</span></span>

<span data-ttu-id="3b34d-118">`IHttpWebSocketFeature`Definuje rozhraní API pro podporu websocket.</span><span class="sxs-lookup"><span data-stu-id="3b34d-118">`IHttpWebSocketFeature` Defines an API for supporting web sockets.</span></span>

<span data-ttu-id="3b34d-119">`IHttpRequestIdentifierFeature`Přidá vlastnost, která může být implementováno k jednoznačné identifikaci požadavků.</span><span class="sxs-lookup"><span data-stu-id="3b34d-119">`IHttpRequestIdentifierFeature` Adds a property that can be implemented to uniquely identify requests.</span></span>

<span data-ttu-id="3b34d-120">`ISessionFeature`Definuje `ISessionFactory` a `ISession` abstrakce pro podporu uživatelských relací.</span><span class="sxs-lookup"><span data-stu-id="3b34d-120">`ISessionFeature` Defines `ISessionFactory` and `ISession` abstractions for supporting user sessions.</span></span>

<span data-ttu-id="3b34d-121">`ITlsConnectionFeature`Definuje rozhraní API pro načítání klientské certifikáty.</span><span class="sxs-lookup"><span data-stu-id="3b34d-121">`ITlsConnectionFeature` Defines an API for retrieving client certificates.</span></span>

<span data-ttu-id="3b34d-122">`ITlsTokenBindingFeature`Definuje metody pro práci s parametry token vazbu protokolu TLS.</span><span class="sxs-lookup"><span data-stu-id="3b34d-122">`ITlsTokenBindingFeature` Defines methods for working with TLS token binding parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="3b34d-123">`ISessionFeature`není funkce serveru, ale je implementováno modulem `SessionMiddleware` (viz [stav aplikace pro správu](app-state.md)).</span><span class="sxs-lookup"><span data-stu-id="3b34d-123">`ISessionFeature` isn't a server feature, but is implemented by the `SessionMiddleware` (see [Managing Application State](app-state.md)).</span></span>

## <a name="feature-collections"></a><span data-ttu-id="3b34d-124">Funkce kolekce</span><span class="sxs-lookup"><span data-stu-id="3b34d-124">Feature collections</span></span>

<span data-ttu-id="3b34d-125">`Features` Vlastnost `HttpContext` poskytuje rozhraní pro získání a nastavení k dispozici funkce protokolu HTTP pro aktuální požadavek.</span><span class="sxs-lookup"><span data-stu-id="3b34d-125">The `Features` property of `HttpContext` provides an interface for getting and setting the available HTTP features for the current request.</span></span> <span data-ttu-id="3b34d-126">Vzhledem k tomu, že kolekce funkce je měnitelný i v kontextu požadavku, middleware slouží k úpravě kolekce a přidání podpory pro další funkce.</span><span class="sxs-lookup"><span data-stu-id="3b34d-126">Since the feature collection is mutable even within the context of a request, middleware can be used to modify the collection and add support for additional features.</span></span>

## <a name="middleware-and-request-features"></a><span data-ttu-id="3b34d-127">Funkce middlewaru a požadavku</span><span class="sxs-lookup"><span data-stu-id="3b34d-127">Middleware and request features</span></span>

<span data-ttu-id="3b34d-128">Servery jsou zodpovědný za vytváření kolekce funkce, middleware můžete přidat do této kolekce i využívat funkce z kolekce.</span><span class="sxs-lookup"><span data-stu-id="3b34d-128">While servers are responsible for creating the feature collection, middleware can both add to this collection and consume features from the collection.</span></span> <span data-ttu-id="3b34d-129">Například `StaticFileMiddleware` přistupuje `IHttpSendFileFeature` funkce.</span><span class="sxs-lookup"><span data-stu-id="3b34d-129">For example, the `StaticFileMiddleware` accesses the `IHttpSendFileFeature` feature.</span></span> <span data-ttu-id="3b34d-130">Pokud funkci existuje, se používá k odesílání požadovaný statických souborů z fyzické cesty.</span><span class="sxs-lookup"><span data-stu-id="3b34d-130">If the feature exists, it's used to send the requested static file from its physical path.</span></span> <span data-ttu-id="3b34d-131">Pomalejší alternativní metodu, jinak se používá k odesílání souboru.</span><span class="sxs-lookup"><span data-stu-id="3b34d-131">Otherwise, a slower alternative method is used to send the file.</span></span> <span data-ttu-id="3b34d-132">Pokud je k dispozici, `IHttpSendFileFeature` umožňuje operačního systému, otevřete soubor a provádět kopie režimu jádra přímé síťové karty.</span><span class="sxs-lookup"><span data-stu-id="3b34d-132">When available, the `IHttpSendFileFeature` allows the operating system to open the file and perform a direct kernel mode copy to the network card.</span></span>

<span data-ttu-id="3b34d-133">Kromě toho middleware můžete přidat do kolekce funkce navázat serverem.</span><span class="sxs-lookup"><span data-stu-id="3b34d-133">Additionally, middleware can add to the feature collection established by the server.</span></span> <span data-ttu-id="3b34d-134">Stávajících funkcí lze nahradit i middleware, což middleware k posílení funkce serveru.</span><span class="sxs-lookup"><span data-stu-id="3b34d-134">Existing features can even be replaced by middleware, allowing the middleware to augment the functionality of the server.</span></span> <span data-ttu-id="3b34d-135">Funkce přidané do kolekce jsou k dispozici okamžitě další middleware nebo základní vlastní aplikace později v kanálu požadavku.</span><span class="sxs-lookup"><span data-stu-id="3b34d-135">Features added to the collection are available immediately to other middleware or the underlying application itself later in the request pipeline.</span></span>

<span data-ttu-id="3b34d-136">Kombinací implementace vlastního serveru a vylepšení určitému middlewaru jde konstruovat přesné sadu funkcí, které aplikace vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="3b34d-136">By combining custom server implementations and specific middleware enhancements, the precise set of features an application requires can be constructed.</span></span> <span data-ttu-id="3b34d-137">To umožňuje chybí funkce, který se má přidat bez nutnosti změny na serveru a zajišťuje jsou viditelné pouze minimální množství funkcí, proto omezení útoku surface area a vylepšuje výkon.</span><span class="sxs-lookup"><span data-stu-id="3b34d-137">This allows missing features to be added without requiring a change in server, and ensures only the minimal amount of features are exposed, thus limiting attack surface area and improving performance.</span></span>

## <a name="summary"></a><span data-ttu-id="3b34d-138">Souhrn</span><span class="sxs-lookup"><span data-stu-id="3b34d-138">Summary</span></span>

<span data-ttu-id="3b34d-139">Funkce rozhraní definovat konkrétní funkce protokolu HTTP, které můžou podporovat daného požadavku.</span><span class="sxs-lookup"><span data-stu-id="3b34d-139">Feature interfaces define specific HTTP features that a given request may support.</span></span> <span data-ttu-id="3b34d-140">Servery definovat kolekce funkcí a počáteční sadu funkcí, které tento server podporuje, ale ke zvýšení tyto funkce můžete použít middleware.</span><span class="sxs-lookup"><span data-stu-id="3b34d-140">Servers define collections of features, and the initial set of features supported by that server, but middleware can be used to enhance these features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3b34d-141">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="3b34d-141">Additional Resources</span></span>

* [<span data-ttu-id="3b34d-142">Servery</span><span class="sxs-lookup"><span data-stu-id="3b34d-142">Servers</span></span>](servers/index.md)

* [<span data-ttu-id="3b34d-143">Middleware</span><span class="sxs-lookup"><span data-stu-id="3b34d-143">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="3b34d-144">Otevřené webové rozhraní pro .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="3b34d-144">Open Web Interface for .NET (OWIN)</span></span>](owin.md)
