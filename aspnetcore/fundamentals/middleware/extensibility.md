---
title: "Aktivace na základě Factory middlewaru v ASP.NET Core"
author: guardrex
description: "Další informace o použití silného typu middlewaru s implementací aktivaci založenou na objektu pro vytváření v ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/29/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 57ff9db2edbf307f2442443dc14e69b0498f7475
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="7bf68-103">Aktivace na základě Factory middlewaru v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7bf68-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="7bf68-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7bf68-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7bf68-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) je bod rozšiřitelnosti pro [middleware](xref:fundamentals/middleware/index) aktivace.</span><span class="sxs-lookup"><span data-stu-id="7bf68-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="7bf68-106">`UseMiddleware`rozšiřující metody zkontrolovat, pokud se implementuje middleware registrovaného typu `IMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="7bf68-106">`UseMiddleware` extension methods check if a middleware's registered type implements `IMiddleware`.</span></span> <span data-ttu-id="7bf68-107">Pokud ano, `IMiddlewareFactory` instance zaregistrován v kontejneru se používá k překladu `IMiddleware` implementace místo použití logiku aktivace založené na konvenci middleware.</span><span class="sxs-lookup"><span data-stu-id="7bf68-107">If it does, the `IMiddlewareFactory` instance registered in the container is used to resolve the `IMiddleware` implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="7bf68-108">Middleware je zaregistrován jako služba vymezenou nebo přechodný v kontejneru služby aplikace.</span><span class="sxs-lookup"><span data-stu-id="7bf68-108">The middleware is registered as a scoped or transient service in the app's service container.</span></span>

<span data-ttu-id="7bf68-109">Výhody:</span><span class="sxs-lookup"><span data-stu-id="7bf68-109">Benefits:</span></span>

* <span data-ttu-id="7bf68-110">Aktivace na základě požadavku (vkládání vymezená služeb)</span><span class="sxs-lookup"><span data-stu-id="7bf68-110">Activation per request (injection of scoped services)</span></span>
* <span data-ttu-id="7bf68-111">Silného typování middlewaru</span><span class="sxs-lookup"><span data-stu-id="7bf68-111">Strong typing of middleware</span></span>

<span data-ttu-id="7bf68-112">`IMiddleware`Každý požadavek, je aktivovaná, takže vymezené služby může vložit do konstruktor middlewaru.</span><span class="sxs-lookup"><span data-stu-id="7bf68-112">`IMiddleware` is activated per request, so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="7bf68-113">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7bf68-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7bf68-114">Ukázková aplikace ukazuje middleware aktivované tímto:</span><span class="sxs-lookup"><span data-stu-id="7bf68-114">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="7bf68-115">Konvence (`ConventionalMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="7bf68-115">Convention (`ConventionalMiddleware`).</span></span> <span data-ttu-id="7bf68-116">Další informace o aktivaci konvenční middleware, najdete v článku [Middleware](xref:fundamentals/middleware/index) tématu.</span><span class="sxs-lookup"><span data-stu-id="7bf68-116">For more information on conventional middleware activation, see the [Middleware](xref:fundamentals/middleware/index) topic.</span></span>
* <span data-ttu-id="7bf68-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) implementace (`IMiddlewareMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="7bf68-117">An [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) implementation (`IMiddlewareMiddleware`).</span></span> <span data-ttu-id="7bf68-118">Výchozí hodnota [MiddlewareFactory třída](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) aktivuje middleware.</span><span class="sxs-lookup"><span data-stu-id="7bf68-118">The default [MiddlewareFactory class](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) activates the middleware.</span></span>

<span data-ttu-id="7bf68-119">Implementace middlewaru fungovat stejně jako a zaznamenejte hodnotu poskytovanou infrastrukturou parametr řetězce dotazu (`key`).</span><span class="sxs-lookup"><span data-stu-id="7bf68-119">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="7bf68-120">Middlewares použijte kontextu vloženého databáze (vymezená service) pro hodnotu řetězce dotazu v databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="7bf68-120">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

## <a name="imiddleware"></a><span data-ttu-id="7bf68-121">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="7bf68-121">IMiddleware</span></span>

<span data-ttu-id="7bf68-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definuje middleware pro aplikace požadavku kanálu.</span><span class="sxs-lookup"><span data-stu-id="7bf68-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="7bf68-123">[InvokeAsync (položka HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) metoda zpracovává požadavky a vrátí `Task` představující provádění middleware.</span><span class="sxs-lookup"><span data-stu-id="7bf68-123">The [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) method handles requests and returns a `Task` that represents the execution of the middleware.</span></span>

<span data-ttu-id="7bf68-124">Middleware aktivovat pomocí konvence:</span><span class="sxs-lookup"><span data-stu-id="7bf68-124">Middleware activated by convention:</span></span>

[!code-csharp[Main](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="7bf68-125">Middleware aktivován `MiddlewareFactory`:</span><span class="sxs-lookup"><span data-stu-id="7bf68-125">Middleware activated by `MiddlewareFactory`:</span></span>

[!code-csharp[Main](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

<span data-ttu-id="7bf68-126">Rozšíření jsou vytvořeny pro middlewares:</span><span class="sxs-lookup"><span data-stu-id="7bf68-126">Extensions are created for the middlewares:</span></span>

[!code-csharp[Main](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="7bf68-127">Není možné předat objekty factory aktivuje middlewaru s `UseMiddleware`:</span><span class="sxs-lookup"><span data-stu-id="7bf68-127">It isn't possible to pass objects to the factory-activated middleware with `UseMiddleware`:</span></span>

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

<span data-ttu-id="7bf68-128">Middleware factory aktivuje je přidat do kontejneru integrované v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7bf68-128">The factory-activated middleware is added to the built-in container in *Startup.cs*:</span></span>

[!code-csharp[Main](extensibility/sample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="7bf68-129">Obě middlewares jsou zaregistrovány v kanálu zpracování požadavku v `Configure`:</span><span class="sxs-lookup"><span data-stu-id="7bf68-129">Both middlewares are registered in the request processing pipeline in `Configure`:</span></span>

[!code-csharp[Main](extensibility/sample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="7bf68-130">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="7bf68-130">IMiddlewareFactory</span></span>

<span data-ttu-id="7bf68-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) poskytuje metody pro vytvoření middleware.</span><span class="sxs-lookup"><span data-stu-id="7bf68-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span> <span data-ttu-id="7bf68-132">Implementace objektu factory middlewaru je zaregistrován v kontejneru jako vymezené služby.</span><span class="sxs-lookup"><span data-stu-id="7bf68-132">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="7bf68-133">Výchozí hodnota `IMiddlewareFactory` implementace [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), se nachází v [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) balíčku ([odkaz na zdroj](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).</span><span class="sxs-lookup"><span data-stu-id="7bf68-133">The default `IMiddlewareFactory` implementation, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package ([reference source](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7bf68-134">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7bf68-134">Additional resources</span></span>

* [<span data-ttu-id="7bf68-135">Middleware</span><span class="sxs-lookup"><span data-stu-id="7bf68-135">Middleware</span></span>](xref:fundamentals/middleware/index)
