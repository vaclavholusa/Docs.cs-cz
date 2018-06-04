---
title: Middleware aktivaci s použitím kontejner třetích stran v ASP.NET Core
author: guardrex
description: Další informace o použití silného typu middleware aktivace prostřednictvím objektu pro vytváření a kontejner třetích stran v ASP.NET Core.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 02/02/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: c55075fd3c6fda4073d26925eab823c35d8656f5
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729889"
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a><span data-ttu-id="64079-103">Middleware aktivaci s použitím kontejner třetích stran v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64079-103">Middleware activation with a third-party container in ASP.NET Core</span></span>

<span data-ttu-id="64079-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="64079-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="64079-105">Tento článek ukazuje, jak používat [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) a [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) jako bod rozšiřitelnosti pro [middleware](xref:fundamentals/middleware/index) aktivaci s použitím kontejner třetích stran.</span><span class="sxs-lookup"><span data-stu-id="64079-105">This article demonstrates how to use [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) and [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) as an extensibility point for [middleware](xref:fundamentals/middleware/index) activation with a third-party container.</span></span> <span data-ttu-id="64079-106">Úvodní informace o `IMiddlewareFactory` a `IMiddleware`, najdete v článku [aktivace na základě Factory middleware](xref:fundamentals/middleware/extensibility) tématu.</span><span class="sxs-lookup"><span data-stu-id="64079-106">For introductory information on `IMiddlewareFactory` and `IMiddleware`, see the [Factory-based middleware activation](xref:fundamentals/middleware/extensibility) topic.</span></span>

<span data-ttu-id="64079-107">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="64079-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="64079-108">Ukázková aplikace ukazuje middleware aktivace pomocí `IMiddlewareFactory` implementace `SimpleInjectorMiddlewareFactory`.</span><span class="sxs-lookup"><span data-stu-id="64079-108">The sample app demonstrates middleware activation by an `IMiddlewareFactory` implementation, `SimpleInjectorMiddlewareFactory`.</span></span> <span data-ttu-id="64079-109">Ukázce se používá [jednoduché Injector](https://simpleinjector.org) kontejneru pro vkládání (DI) závislosti.</span><span class="sxs-lookup"><span data-stu-id="64079-109">The sample uses the [Simple Injector](https://simpleinjector.org) dependency injection (DI) container.</span></span>

<span data-ttu-id="64079-110">Implementace tohoto příkladu middleware zaznamenává hodnotu poskytovanou infrastrukturou parametr řetězce dotazu (`key`).</span><span class="sxs-lookup"><span data-stu-id="64079-110">The sample's middleware implementation records the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="64079-111">Middleware kontextu vloženého databáze (vymezená service) používá k zaznamenání hodnotu řetězce dotazu v databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="64079-111">The middleware uses an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

> [!NOTE]
> <span data-ttu-id="64079-112">Použití ukázkové aplikace [jednoduché Injector](https://github.com/simpleinjector/SimpleInjector) výhradně pro demonstrační účely.</span><span class="sxs-lookup"><span data-stu-id="64079-112">The sample app uses [Simple Injector](https://github.com/simpleinjector/SimpleInjector) purely for demonstration purposes.</span></span> <span data-ttu-id="64079-113">Použití jednoduchého Injector není o potvrzení.</span><span class="sxs-lookup"><span data-stu-id="64079-113">Use of Simple Injector isn't an endorsement.</span></span> <span data-ttu-id="64079-114">Middleware aktivace přístupy popsaného v dokumentaci k jednoduché Injector a údržby programu z jednoduchého Injector jsou doporučení Githubu problémy.</span><span class="sxs-lookup"><span data-stu-id="64079-114">Middleware activation approaches described in the Simple Injector documentation and GitHub issues are recommended by the maintainers of Simple Injector.</span></span> <span data-ttu-id="64079-115">Další informace najdete v tématu [dokumentace jednoduché Injector](https://simpleinjector.readthedocs.io/en/latest/index.html) a [úložiště GitHub jednoduché Injector](https://github.com/simpleinjector/SimpleInjector).</span><span class="sxs-lookup"><span data-stu-id="64079-115">For more information, see the [Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) and [Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector).</span></span>

## <a name="imiddlewarefactory"></a><span data-ttu-id="64079-116">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="64079-116">IMiddlewareFactory</span></span>

<span data-ttu-id="64079-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) poskytuje metody pro vytvoření middleware.</span><span class="sxs-lookup"><span data-stu-id="64079-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span>

<span data-ttu-id="64079-118">V ukázkové aplikace, se implementuje objekt pro vytváření middleware vytvořit `SimpleInjectorActivatedMiddleware` instance.</span><span class="sxs-lookup"><span data-stu-id="64079-118">In the sample app, a middleware factory is implemented to create an `SimpleInjectorActivatedMiddleware` instance.</span></span> <span data-ttu-id="64079-119">Objekt factory middleware používá jednoduchý Injector kontejner přeložit middleware:</span><span class="sxs-lookup"><span data-stu-id="64079-119">The middleware factory uses the Simple Injector container to resolve the middleware:</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a><span data-ttu-id="64079-120">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="64079-120">IMiddleware</span></span>

<span data-ttu-id="64079-121">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definuje middleware pro aplikace požadavku kanálu.</span><span class="sxs-lookup"><span data-stu-id="64079-121">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span>

<span data-ttu-id="64079-122">Middleware aktivován `IMiddlewareFactory` implementace (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span><span class="sxs-lookup"><span data-stu-id="64079-122">Middleware activated by an `IMiddlewareFactory` implementation (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="64079-123">Vytvoření rozšíření pro middleware (*Middleware/MiddlewareExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="64079-123">An extension is created for the middleware (*Middleware/MiddlewareExtensions.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="64079-124">`Startup.ConfigureServices` musíte provést několik úloh:</span><span class="sxs-lookup"><span data-stu-id="64079-124">`Startup.ConfigureServices` must perform several tasks:</span></span>

* <span data-ttu-id="64079-125">Nastavte jednoduché Injector kontejneru.</span><span class="sxs-lookup"><span data-stu-id="64079-125">Set up the Simple Injector container.</span></span>
* <span data-ttu-id="64079-126">Zaregistrujte objekt pro vytváření a middleware.</span><span class="sxs-lookup"><span data-stu-id="64079-126">Register the factory and middleware.</span></span>
* <span data-ttu-id="64079-127">Zpřístupnit kontext databáze aplikace z kontejneru jednoduché Injector pro stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="64079-127">Make the app's database context available from the Simple Injector container for a Razor Page.</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="64079-128">Middleware je zaregistrován v kanálu zpracování požadavku v `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="64079-128">The middleware is registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet2&highlight=13)]

## <a name="additional-resources"></a><span data-ttu-id="64079-129">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="64079-129">Additional resources</span></span>

* [<span data-ttu-id="64079-130">Middleware</span><span class="sxs-lookup"><span data-stu-id="64079-130">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="64079-131">Aktivace na základě Factory middlewaru</span><span class="sxs-lookup"><span data-stu-id="64079-131">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="64079-132">Jednoduché úložiště Injector GitHub</span><span class="sxs-lookup"><span data-stu-id="64079-132">Simple Injector GitHub repository</span></span>](https://github.com/simpleinjector/SimpleInjector)
* [<span data-ttu-id="64079-133">Jednoduché Injector dokumentace</span><span class="sxs-lookup"><span data-stu-id="64079-133">Simple Injector documentation</span></span>](https://simpleinjector.readthedocs.io/en/latest/index.html)
