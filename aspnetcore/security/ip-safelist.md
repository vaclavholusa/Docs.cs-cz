---
title: Safelist IP klienta pro ASP.NET Core
author: damienbod
description: Další informace o zápisu Middleware nebo akce filtry k ověření vzdálené IP adresy na seznam schválených IP adres.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 40fe7b67359efd1692490099c3fb529ba4a6148f
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040122"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="c6a19-103">Safelist IP klienta pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c6a19-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="c6a19-104">Podle [Damien překážka](https://twitter.com/damien_bod) a [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c6a19-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="c6a19-105">Tento článek ukazuje dva způsoby, jak implementovat safelist IP (označované také jako seznam povolených):</span><span class="sxs-lookup"><span data-stu-id="c6a19-105">This article shows two ways to implement an IP safelist (also known as a whitelist):</span></span>

* <span data-ttu-id="c6a19-106">Pomocí ASP.NET Core middlewarem ke kontrole vzdálené IP adresy každé žádosti o.</span><span class="sxs-lookup"><span data-stu-id="c6a19-106">By using ASP.NET Core middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="c6a19-107">Pomocí filtrů Akce ASP.NET Core ke kontrole Vzdálená IP adresa žádosti o konkrétní akci metody.</span><span class="sxs-lookup"><span data-stu-id="c6a19-107">By using ASP.NET Core action filters to check the remote IP address of requests for specific action methods.</span></span>

<span data-ttu-id="c6a19-108">Ukázková aplikace ukazuje oba přístupy.</span><span class="sxs-lookup"><span data-stu-id="c6a19-108">The sample app illustrates both approaches.</span></span> <span data-ttu-id="c6a19-109">V obou případech je uložen řetězec obsahující schválených klientských IP adres v nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="c6a19-109">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="c6a19-110">Middleware nebo filtr analyzuje řetězec do seznam a zkontroluje, zda vzdálené IP je v seznamu.</span><span class="sxs-lookup"><span data-stu-id="c6a19-110">The middleware or filter parses the string into a list and  checks if the remote IP is in the list.</span></span> <span data-ttu-id="c6a19-111">V opačném případě se vrátí stavový kód HTTP 403 Zakázáno.</span><span class="sxs-lookup"><span data-stu-id="c6a19-111">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="c6a19-112">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c6a19-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="c6a19-113">Safelist</span><span class="sxs-lookup"><span data-stu-id="c6a19-113">The safelist</span></span>

<span data-ttu-id="c6a19-114">V seznamu je nakonfigurovaný v *appsettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="c6a19-114">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="c6a19-115">Středníkem oddělený seznam a může obsahovat adresy IPv4 a IPv6.</span><span class="sxs-lookup"><span data-stu-id="c6a19-115">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="c6a19-116">Middleware</span><span class="sxs-lookup"><span data-stu-id="c6a19-116">Middleware</span></span>

<span data-ttu-id="c6a19-117">`Configure` Metoda přidá middleware a předá řetězec safelist v parametr konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="c6a19-117">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

<span data-ttu-id="c6a19-118">Middleware analyzuje řetězec do pole a hledá vzdálenou IP adresu v poli.</span><span class="sxs-lookup"><span data-stu-id="c6a19-118">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="c6a19-119">Pokud není nalezena Vzdálená IP adresa, middleware vrátí HTTP 401 zakázáno.</span><span class="sxs-lookup"><span data-stu-id="c6a19-119">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="c6a19-120">Tento proces ověřování přeskočí pro požadavky HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="c6a19-120">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="c6a19-121">Akce filtru</span><span class="sxs-lookup"><span data-stu-id="c6a19-121">Action filter</span></span>

<span data-ttu-id="c6a19-122">Pokud chcete safelist pouze pro metody akce nebo konkrétní řadiče, použijte filtr akce.</span><span class="sxs-lookup"><span data-stu-id="c6a19-122">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="c6a19-123">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="c6a19-123">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="c6a19-124">Filtr akce se přidá do kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="c6a19-124">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="c6a19-125">Filtr lze pak použít v kontroleru nebo metodě akce.</span><span class="sxs-lookup"><span data-stu-id="c6a19-125">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="c6a19-126">V ukázkové aplikaci filtr platí pro `Get` metody.</span><span class="sxs-lookup"><span data-stu-id="c6a19-126">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="c6a19-127">Ano, při testování aplikace pomocí odesílání `Get` žádosti rozhraní API, atribut ověřuje IP adresu klienta.</span><span class="sxs-lookup"><span data-stu-id="c6a19-127">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="c6a19-128">Při testování voláním rozhraní API pomocí jiné metody HTTP, middleware ověřování IP adresu klienta.</span><span class="sxs-lookup"><span data-stu-id="c6a19-128">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="c6a19-129">Filtr stránek Razor</span><span class="sxs-lookup"><span data-stu-id="c6a19-129">Razor Pages filter</span></span> 

<span data-ttu-id="c6a19-130">Pokud chcete safelist pro aplikace Razor Pages, použijte filtr stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="c6a19-130">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="c6a19-131">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="c6a19-131">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="c6a19-132">Tento filtr je povoleno přidáním do kolekce filtrů MVC.</span><span class="sxs-lookup"><span data-stu-id="c6a19-132">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="c6a19-133">Při spuštění aplikace a požádat o stránku Razor, filtr stránky Razor ověřuje IP adresu klienta.</span><span class="sxs-lookup"><span data-stu-id="c6a19-133">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6a19-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6a19-134">Next steps</span></span>

<span data-ttu-id="c6a19-135">[Další informace o ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="c6a19-135">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
