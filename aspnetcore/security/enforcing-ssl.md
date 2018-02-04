---
title: "Vynucování SSL v aplikaci ASP.NET Core"
author: rick-anderson
description: "Ukazuje, jak chcete vyžadovat protokol SSL v ASP.NET Core webové aplikace"
manager: wpickett
ms.author: riande
ms.date: 07/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 3b72cddb7a240ad6d6e1427796e9bb4f7003a3f7
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/03/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="065c3-103">Vynucování SSL v aplikaci ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="065c3-103">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="065c3-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="065c3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="065c3-105">Tento dokument ukazuje, jak:</span><span class="sxs-lookup"><span data-stu-id="065c3-105">This document shows how to:</span></span>

- <span data-ttu-id="065c3-106">Vyžadovat protokol SSL pro všechny požadavky (pouze požadavky HTTPS).</span><span class="sxs-lookup"><span data-stu-id="065c3-106">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="065c3-107">Přesměrujte všechny požadavky HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="065c3-107">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="065c3-108">Požadovat protokol SSL</span><span class="sxs-lookup"><span data-stu-id="065c3-108">Require SSL</span></span>

<span data-ttu-id="065c3-109">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) slouží k vyžadování protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="065c3-109">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="065c3-110">Můžete uspořádání řadiče nebo metody s tímto atributem nebo ho můžete použít globálně, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="065c3-110">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="065c3-111">Přidejte následující kód, který `ConfigureServices` v `Startup`:</span><span class="sxs-lookup"><span data-stu-id="065c3-111">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="065c3-112">Zvýrazněný výše vyžaduje všechny požadavky používat `HTTPS`, proto jsou ignorovány požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="065c3-112">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="065c3-113">Následující zvýrazněný kód přesměruje všech požadavků HTTP do HTTPS:</span><span class="sxs-lookup"><span data-stu-id="065c3-113">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="065c3-114">V tématu [URL přepisování Middleware](xref:fundamentals/url-rewriting) Další informace.</span><span class="sxs-lookup"><span data-stu-id="065c3-114">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="065c3-115">Globálně vyžadujících protokol HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) je nejlepším postupem zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="065c3-115">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="065c3-116">Použití `[RequireHttps]` atribut, aby všechny řadiče se nepovažuje za bezpečnou jako jako globální vyžadujících protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="065c3-116">Applying the `[RequireHttps]` attribute to all controller isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="065c3-117">Nebudete moct zaručit, nových řadičů přidán do vaší aplikace bude nezapomeňte použít `[RequireHttps]` atribut.</span><span class="sxs-lookup"><span data-stu-id="065c3-117">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
