---
title: "Vynucování HTTPS v aplikaci ASP.NET Core"
author: rick-anderson
description: "Ukazuje, jak chcete vyžadovat protokol HTTPS/TLS v ASP.NET Core webové aplikace."
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 636077ea21581716308384ebf8d47c1e417a256a
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/11/2018
---
# <a name="enforcing-https-in-an-aspnet-core-app"></a><span data-ttu-id="a34f4-103">Vynucování HTTPS v aplikaci ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a34f4-103">Enforcing HTTPS in an ASP.NET Core app</span></span>

<span data-ttu-id="a34f4-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a34f4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a34f4-105">Tento dokument ukazuje, jak:</span><span class="sxs-lookup"><span data-stu-id="a34f4-105">This document shows how to:</span></span>

- <span data-ttu-id="a34f4-106">Vyžadovat protokol HTTPS pro všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="a34f4-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="a34f4-107">Přesměrujte všechny požadavky HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a34f4-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="a34f4-108">Proveďte **není** použít `RequireHttpsAttribute` na webovým rozhraním API, která zobrazit citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="a34f4-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="a34f4-109">`RequireHttpsAttribute`stavové kódy HTTP se používá k přesměrování prohlížeče z HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a34f4-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="a34f4-110">Rozhraní API klienti nemusí pochopit a orientují přesměrování z HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a34f4-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="a34f4-111">Tito klienti mohou odesílat informace přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="a34f4-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="a34f4-112">Webová rozhraní API se musí buď:</span><span class="sxs-lookup"><span data-stu-id="a34f4-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="a34f4-113">Nelze naslouchat na protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a34f4-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="a34f4-114">Ukončení připojení se stavovým kódem 400 (Chybný požadavek) a není požadavek vyřídit.</span><span class="sxs-lookup"><span data-stu-id="a34f4-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="a34f4-115">Vyžadovat protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="a34f4-115">Require HTTPS</span></span>

<span data-ttu-id="a34f4-116">[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) se používá k vyžadují protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a34f4-116">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="a34f4-117">`[RequireHttpsAttribute]`můžete uspořádání řadiče nebo metod, nebo mohou být použity globálně.</span><span class="sxs-lookup"><span data-stu-id="a34f4-117">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="a34f4-118">Použít atribut globálně, přidejte následující kód do `ConfigureServices` v `Startup`:</span><span class="sxs-lookup"><span data-stu-id="a34f4-118">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="a34f4-119">Předchozí zvýrazněný vyžaduje všechny požadavky používat `HTTPS`; proto požadavky HTTP jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="a34f4-119">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="a34f4-120">Následující zvýrazněný kód přesměruje všech požadavků HTTP do HTTPS:</span><span class="sxs-lookup"><span data-stu-id="a34f4-120">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="a34f4-121">Další informace najdete v tématu [URL přepisování Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="a34f4-121">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="a34f4-122">Globálně vyžadujících protokol HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) je nejlepším postupem zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="a34f4-122">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="a34f4-123">Použití `[RequireHttps]` atribut na všechny řadiče nebo Razor stránky se nepovažuje za bezpečnou jako jako globální vyžadujících protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a34f4-123">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="a34f4-124">Nebudete moct zaručit `[RequireHttps]` atribut se používá, když se přidají nové řadiče a stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="a34f4-124">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>