---
title: "Vynucování SSL v aplikaci ASP.NET Core"
author: rick-anderson
description: "Ukazuje, jak chcete vyžadovat protokol SSL v ASP.NET Core webové aplikace"
keywords: "ASP.NET Core, SSL, HTTPS, RequireHttpsAttribute, službě IIS Express"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 6f2755a606000717ca8a57f045b1ef613c7f14f6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="d94e4-104">Vynucování SSL v aplikaci ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d94e4-104">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="d94e4-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d94e4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d94e4-106">Tento dokument ukazuje, jak:</span><span class="sxs-lookup"><span data-stu-id="d94e4-106">This document shows how to:</span></span>

- <span data-ttu-id="d94e4-107">Vyžadovat protokol SSL pro všechny požadavky (pouze požadavky HTTPS).</span><span class="sxs-lookup"><span data-stu-id="d94e4-107">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="d94e4-108">Přesměrujte všechny požadavky HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d94e4-108">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="d94e4-109">Požadovat protokol SSL</span><span class="sxs-lookup"><span data-stu-id="d94e4-109">Require SSL</span></span>

<span data-ttu-id="d94e4-110">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) slouží k vyžadování protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="d94e4-110">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="d94e4-111">Můžete uspořádání řadiče nebo metody s tímto atributem nebo ho můžete použít globálně, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="d94e4-111">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="d94e4-112">Přidejte následující kód, který `ConfigureServices` v `Startup`:</span><span class="sxs-lookup"><span data-stu-id="d94e4-112">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="d94e4-113">Zvýrazněný výše vyžaduje všechny požadavky používat `HTTPS`, proto jsou ignorovány požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="d94e4-113">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="d94e4-114">Následující zvýrazněný kód přesměruje všech požadavků HTTP do HTTPS:</span><span class="sxs-lookup"><span data-stu-id="d94e4-114">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="d94e4-115">V tématu [URL přepisování Middleware](xref:fundamentals/url-rewriting) Další informace.</span><span class="sxs-lookup"><span data-stu-id="d94e4-115">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="d94e4-116">Globálně vyžadujících protokol HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) je nejlepším postupem zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="d94e4-116">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="d94e4-117">Použití `[RequireHttps]` atribut, aby všechny řadiče se nepovažuje za bezpečnou jako jako globální vyžadujících protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d94e4-117">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="d94e4-118">Nebudete moct zaručit, nových řadičů přidán do vaší aplikace bude nezapomeňte použít `[RequireHttps]` atribut.</span><span class="sxs-lookup"><span data-stu-id="d94e4-118">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
