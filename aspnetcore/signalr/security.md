---
title: Informace o zabezpečení ve funkci SignalR technologie ASP.NET Core
author: tdykstra
description: Další informace o použití ověřování a autorizace v knihovně SignalR technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: 98b5eb7be87920aacf7a941f76ff652ae7905303
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391255"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="ca6fe-103">Informace o zabezpečení ve funkci SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca6fe-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="ca6fe-104">Podle [Andrew Stanton sestry](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="ca6fe-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

## <a name="overview"></a><span data-ttu-id="ca6fe-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="ca6fe-105">Overview</span></span>

<span data-ttu-id="ca6fe-106">Funkce SignalR poskytuje řadu ochranu zabezpečení ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-106">SignalR provides a number of security protections by default.</span></span> <span data-ttu-id="ca6fe-107">Je důležité pochopit, jak nakonfigurovat tyto ochrany.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-107">It's important to understand how to configure these protections.</span></span>

### <a name="cross-origin-resource-sharing"></a><span data-ttu-id="ca6fe-108">Sdílení prostředků různého původu</span><span class="sxs-lookup"><span data-stu-id="ca6fe-108">Cross-origin resource sharing</span></span>

<span data-ttu-id="ca6fe-109">[Prostředků mezi zdroji (CORS) pro sdílení obsahu](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) slouží k povolení připojení SignalR nepůvodního zdroje v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-109">[Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="ca6fe-110">Pokud váš kód jazyka JavaScript je hostovaný na jiný název domény z vaší aplikace SignalR, budete muset povolit [middlewarem ASP.NET Core CORS](xref:security/cors) umožní připojení.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-110">If your JavaScript code is hosted on a different domain name from your SignalR app, you have to enable the [ASP.NET Core CORS middleware](xref:security/cors) in order to allow the connection.</span></span> <span data-ttu-id="ca6fe-111">Obecně platí povolení žádostí nepůvodního pouze z domén, které spravujete.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-111">In general, allow cross-origin requests only from domains you control.</span></span> <span data-ttu-id="ca6fe-112">Například, pokud je hostitelem vašeho webu `http://www.example.com` a je hostitelem vaší aplikace SignalR `http://signalr.example.com`, byste měli nakonfigurovat CORS ve vaší aplikaci SignalR a Povolit jenom původ `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-112">For example, if your site is hosted on `http://www.example.com` and your SignalR app is hosted on `http://signalr.example.com`, you should configure CORS in your SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="ca6fe-113">Další informace o konfiguraci CORS, najdete v části [dokumentaci k ASP.NET Core CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="ca6fe-113">For more information on configuring CORS, see [the documentation on ASP.NET Core CORS](xref:security/cors).</span></span> <span data-ttu-id="ca6fe-114">Funkce SignalR správnou funkci vyžaduje následující zásady CORS:</span><span class="sxs-lookup"><span data-stu-id="ca6fe-114">SignalR requires the following CORS policies in order to operate correctly:</span></span>

* <span data-ttu-id="ca6fe-115">Zásada musí povolit konkrétní zdroje očekávat, nebo povolíte původu (nedoporučuje se).</span><span class="sxs-lookup"><span data-stu-id="ca6fe-115">The policy must allow the specific origins you expect, or allow any origin (not recommended).</span></span>
* <span data-ttu-id="ca6fe-116">Metody HTTP `GET` a `POST` musí být povoleno.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-116">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="ca6fe-117">Přihlašovací údaje musí být povolena, i když nepoužíváte ověřování.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-117">Credentials must be enabled, even when you aren't using authentication.</span></span>

<span data-ttu-id="ca6fe-118">Například následující zásadu CORS umožňuje klientovi SignalR prohlížeče hostované na `http://example.com` pro přístup k vaší aplikace SignalR:</span><span class="sxs-lookup"><span data-stu-id="ca6fe-118">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access your SignalR app:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> <span data-ttu-id="ca6fe-119">SignalR je nekompatibilní s integrovaná funkce CORS v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-119">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

### <a name="websocket-origin-restriction"></a><span data-ttu-id="ca6fe-120">Omezení objektu websocket na straně zdroje</span><span class="sxs-lookup"><span data-stu-id="ca6fe-120">WebSocket Origin Restriction</span></span>

<span data-ttu-id="ca6fe-121">Poskytovanou CORS se nevztahují na objekty Websocket.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-121">The protections provided by CORS do not apply to WebSockets.</span></span> <span data-ttu-id="ca6fe-122">Prohlížeče neprovádějte požadavků CORS přípravné ani provést respektovat zadaná v omezení `Access-Control` záhlaví při provádění požadavků protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-122">Browsers do not perform CORS pre-flight requests, nor do they respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span> <span data-ttu-id="ca6fe-123">Ale prohlížeče odesílají `Origin` záhlaví při vydávání žádostí protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-123">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="ca6fe-124">Měli byste nakonfigurovat aplikace k ověření tyto hlavičky, aby bylo možné zajistit, aby pocházející ze streamovacího zdroje, které očekáváte, že jsou povoleny pouze objekty Websocket.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-124">You should configure your application to validate these headers in order to ensure that only WebSockets coming from the origins you expect are allowed.</span></span>

<span data-ttu-id="ca6fe-125">V ASP.NET Core 2.1 toho lze dosáhnout pomocí vlastního middlewaru, který můžete umístit **nad `UseSignalR`a veškerý middleware ověřování** ve vaší `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="ca6fe-125">In ASP.NET Core 2.1, this can be achieved using a custom middleware you can place **above `UseSignalR`, and any authentication middleware** in your `Configure` method:</span></span>

```csharp
// In your Startup class, add a static field listing the allowed Origin values:
private static readonly HashSet<string> _allowedOrigins = new HashSet<string>()
{
    // Add allowed origins here. For example:
    "http://www.mysite.com",
    "http://mysite.com",
};

// In your Configure method:
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Validate Origin header on WebSocket requests to prevent unexpected cross-site WebSocket requests
    app.Use((context, next) =>
    {
        // Check for a WebSocket request.
        if(string.Equals(context.Request.Headers["Upgrade"], "websocket"))
        {
            var origin = context.Request.Headers["Origin"];

            // If there is no origin header, or if the origin header doesn't match an allowed value:
            if(string.IsNullOrEmpty(origin) && !_allowedOrigins.Contains(origin))
            {
                // The origin is not allowed, reject the request
                context.Response.StatusCode = StatusCodes.Status400BadRequest;
                return Task.CompletedTask;
            }
        }

        // The request is not a WebSocket request or is a valid Origin, so let it continue
        return next();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> <span data-ttu-id="ca6fe-126">`Origin` Záhlaví je řízen zcela klienta a, podobně jako `Referer` záhlaví, můžete zfalšovaná.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-126">The `Origin` header is completely controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="ca6fe-127">Tyto hlavičky byste nikdy neměli používat jako mechanismus ověřování.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-127">These headers should never be used as an authentication mechanism.</span></span>

### <a name="access-token-logging"></a><span data-ttu-id="ca6fe-128">Protokolování token přístupu</span><span class="sxs-lookup"><span data-stu-id="ca6fe-128">Access token logging</span></span>

<span data-ttu-id="ca6fe-129">Při použití Server-Sent události nebo protokoly Websocket, klientský prohlížeč odesílá přístupový token v řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-129">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="ca6fe-130">Toto je obecně stejně bezpečné jako použití standardní `Authorization` záhlaví, ale mnohé webové servery protokolu pro každý požadavek adresu URL, včetně řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-130">This is generally as secure as using the standard `Authorization` header, however many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="ca6fe-131">To znamená, že přístupový token může být součástí protokolů.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-131">This means the access token may be included in logs.</span></span> <span data-ttu-id="ca6fe-132">Vezměte v úvahu zkontrolujete nastavení protokolování webového serveru, aby tyto informace protokolování.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-132">Consider reviewing the web server's logging settings to avoid logging this information.</span></span>

### <a name="exceptions"></a><span data-ttu-id="ca6fe-133">Výjimky</span><span class="sxs-lookup"><span data-stu-id="ca6fe-133">Exceptions</span></span>

<span data-ttu-id="ca6fe-134">Zprávy o výjimkách jsou obvykle považovány za citlivá data, která by neměla být odhalena do klienta.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-134">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="ca6fe-135">Ve výchozím nastavení nebude funkce SignalR Odeslat podrobnosti výjimky vyvolané metodou rozbočovače klientovi.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-135">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="ca6fe-136">Místo toho klient obdrží obecná zpráva oznamující, že došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-136">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="ca6fe-137">Toto chování můžete přepsat tak, že nastavíte [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) nastavení.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-137">You can override this behavior by setting the [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options) setting.</span></span>

### <a name="buffer-management"></a><span data-ttu-id="ca6fe-138">Správa vyrovnávací paměti</span><span class="sxs-lookup"><span data-stu-id="ca6fe-138">Buffer management</span></span>

<span data-ttu-id="ca6fe-139">Funkce SignalR používá vyrovnávací paměti za připojení za účelem správy příchozí a odchozí zprávy.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-139">SignalR uses per-connection buffers in order to manage incoming and outgoing messages.</span></span> <span data-ttu-id="ca6fe-140">Ve výchozím nastavení omezuje SignalR vyrovnávací paměti na 32KB.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-140">By default, SignalR limits these buffers to 32KB.</span></span> <span data-ttu-id="ca6fe-141">To znamená, že největší možné zprávu, kterou můžete poslat klient nebo server je 32 KB.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-141">This means the largest possible message a client or server can send is 32KB.</span></span> <span data-ttu-id="ca6fe-142">Zároveň to znamená, že maximální množství paměti používané připojení pro zprávy je 32 KB.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-142">This also means the maximum amount of memory consumed by a connection for messages is 32KB.</span></span> <span data-ttu-id="ca6fe-143">Pokud víte, že vaše zprávy jsou vždy menší než toto omezení, můžete snížit velikost zabránit odeslání větší zprávy a vynutit serveru přidělení paměti přijmout klienta.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-143">If you know your messages are always smaller than this limit, you can reduce this size to prevent a client from being able to send a larger message and force the server to allocate memory to accept it.</span></span> <span data-ttu-id="ca6fe-144">Podobně pokud víte, že vaše zprávy jsou větší než toto omezení, můžete zvýšit ji.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-144">Similarly, if you know your messages are larger than this limit, you can increase it.</span></span> <span data-ttu-id="ca6fe-145">Nezapomínejte, že tento limit zvýšit znamená, že klient může způsobit, že server další paměť přidělit a může snížit počet souběžných připojení, které vaše aplikace dokáže zpracovat.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-145">However, be aware that increasing this limit means that the client is able to cause the server to allocate additional memory and may reduce the number of concurrent connections your app can handle.</span></span>

<span data-ttu-id="ca6fe-146">Existují zvláštní omezení pro příchozí a odchozí zprávy, jak lze nakonfigurovat podle [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) objekt gurovaný `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="ca6fe-146">There are separate limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="ca6fe-147">`ApplicationMaxBufferSize` představuje maximální počet bajtů z klienta, který vyrovnávací paměti serveru.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-147">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="ca6fe-148">Pokud se klient pokusí odeslat zprávu větší než tento limit, může připojení ukončeno.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-148">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="ca6fe-149">`TransportMaxBufferSize` představuje maximální počet bajtů, které může server odeslat.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-149">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="ca6fe-150">Pokud se server pokusí odeslat zprávu (zahrnuje návratové hodnoty metod rozbočovače na) větší než tento limit, bude vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-150">If the server attempts to send a message (includes return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="ca6fe-151">Nastavení limitu `0` zcela zakazuje limit.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-151">Setting the limit to `0` disables the limit entirely.</span></span> <span data-ttu-id="ca6fe-152">Ale to by mělo být provedeno s nejvyšší opatrností.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-152">However, this should be done with extreme caution.</span></span> <span data-ttu-id="ca6fe-153">Odebrání limitu umožňuje klientovi umožní odeslat zprávu libovolné velikosti.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-153">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="ca6fe-154">To lze použít škodlivého klienta způsobit nadbytek paměti, kterou je potřeba přidělit, což může výrazně snížit počet souběžných připojení, které vaše aplikace může podporovat.</span><span class="sxs-lookup"><span data-stu-id="ca6fe-154">This could be used by a malicious client to cause excess memory to be allocated, which could dramatically reduce the number of concurrent connections your app can support.</span></span>
