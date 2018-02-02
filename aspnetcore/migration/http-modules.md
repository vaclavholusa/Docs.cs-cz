---
title: "Migrace obslužné rutiny HTTP a moduly, které middleware ASP.NET Core"
author: rick-anderson
description: 
manager: wpickett
ms.author: tdykstra
ms.date: 12/07/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/http-modules
ms.openlocfilehash: 8aac6c649b22dc8f6cfc916aa78d56efad7821a0
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="a5d39-102">Migrace obslužné rutiny HTTP a moduly, které middleware ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5d39-102">Migrating HTTP handlers and modules to ASP.NET Core middleware</span></span> 

<span data-ttu-id="a5d39-103">Podle [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span><span class="sxs-lookup"><span data-stu-id="a5d39-103">By [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span></span>

<span data-ttu-id="a5d39-104">Tento článek ukazuje, jak migrovat existující ASP.NET [modulů HTTP a obslužné rutiny z system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/) na jádro ASP.NET [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="a5d39-104">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/) to ASP.NET Core [middleware](xref:fundamentals/middleware/index).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="a5d39-105">Moduly a obslužné rutiny kdykoli znovu spustit.</span><span class="sxs-lookup"><span data-stu-id="a5d39-105">Modules and handlers revisited</span></span>

<span data-ttu-id="a5d39-106">Před pokračováním ASP.NET Core middleware, můžeme nejprve recap jak fungují moduly HTTP a obslužné rutiny:</span><span class="sxs-lookup"><span data-stu-id="a5d39-106">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![Obslužná rutina moduly](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="a5d39-108">**Obslužné rutiny jsou:**</span><span class="sxs-lookup"><span data-stu-id="a5d39-108">**Handlers are:**</span></span>

   * <span data-ttu-id="a5d39-109">Třídy implementující [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span><span class="sxs-lookup"><span data-stu-id="a5d39-109">Classes that implement [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span></span>

   * <span data-ttu-id="a5d39-110">Umožňuje zpracování požadavků daného názvu souboru nebo rozšíření, jako například *.report*</span><span class="sxs-lookup"><span data-stu-id="a5d39-110">Used to handle requests with a given file name or extension, such as *.report*</span></span>

   * <span data-ttu-id="a5d39-111">[Nakonfigurované](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) v *souboru Web.config*</span><span class="sxs-lookup"><span data-stu-id="a5d39-111">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="a5d39-112">**Moduly jsou:**</span><span class="sxs-lookup"><span data-stu-id="a5d39-112">**Modules are:**</span></span>

   * <span data-ttu-id="a5d39-113">Třídy implementující [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span><span class="sxs-lookup"><span data-stu-id="a5d39-113">Classes that implement [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span></span>

   * <span data-ttu-id="a5d39-114">Volá se pro každý požadavek</span><span class="sxs-lookup"><span data-stu-id="a5d39-114">Invoked for every request</span></span>

   * <span data-ttu-id="a5d39-115">Může krátká smyčka (zastaví další zpracování požadavku)</span><span class="sxs-lookup"><span data-stu-id="a5d39-115">Able to short-circuit (stop further processing of a request)</span></span>

   * <span data-ttu-id="a5d39-116">Možné přidat do odpovědi HTTP ani vytvářet své vlastní</span><span class="sxs-lookup"><span data-stu-id="a5d39-116">Able to add to the HTTP response, or create their own</span></span>

   * <span data-ttu-id="a5d39-117">[Nakonfigurované](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) v *souboru Web.config*</span><span class="sxs-lookup"><span data-stu-id="a5d39-117">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="a5d39-118">**Pořadí modulů zpracovat příchozí žádosti, ve kterém je určený:**</span><span class="sxs-lookup"><span data-stu-id="a5d39-118">**The order in which modules process incoming requests is determined by:**</span></span>

   1. <span data-ttu-id="a5d39-119">[Životního cyklu aplikace](https://msdn.microsoft.com/library/ms227673.aspx), což je řady událostí, aktivováno technologií ASP.NET: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest)atd. Každý modul můžete vytvořit obslužnou rutinu pro jeden nebo více událostí.</span><span class="sxs-lookup"><span data-stu-id="a5d39-119">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

   2. <span data-ttu-id="a5d39-120">Pro stejnou událost, pořadí, ve které jste nakonfigurovali v *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="a5d39-120">For the same event, the order in which they're configured in *Web.config*.</span></span>

<span data-ttu-id="a5d39-121">Kromě moduly, obslužné rutiny pro události životního cyklu, které přidáte vaše *Global.asax.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="a5d39-121">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="a5d39-122">V těchto obslužných rutinách spustit po obslužné rutiny v nakonfigurované moduly.</span><span class="sxs-lookup"><span data-stu-id="a5d39-122">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="a5d39-123">Z obslužné rutiny a moduly, které middleware</span><span class="sxs-lookup"><span data-stu-id="a5d39-123">From handlers and modules to middleware</span></span>

<span data-ttu-id="a5d39-124">**Middleware jsou jednodušší než vytváření modulů HTTP a obslužné rutiny:**</span><span class="sxs-lookup"><span data-stu-id="a5d39-124">**Middleware are simpler than HTTP modules and handlers:**</span></span>

   * <span data-ttu-id="a5d39-125">Moduly, obslužné rutiny, *Global.asax.cs*, *Web.config* (s výjimkou konfigurace služby IIS) a životního cyklu aplikace jsou pryč</span><span class="sxs-lookup"><span data-stu-id="a5d39-125">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

   * <span data-ttu-id="a5d39-126">Role modulů a obslužné rutiny byly převzaty middlewaru.</span><span class="sxs-lookup"><span data-stu-id="a5d39-126">The roles of both modules and handlers have been taken over by middleware</span></span>

   * <span data-ttu-id="a5d39-127">Middleware jsou konfigurováni pomocí kódu spíše než v *souboru Web.config*</span><span class="sxs-lookup"><span data-stu-id="a5d39-127">Middleware are configured using code rather than in *Web.config*</span></span>

   * <span data-ttu-id="a5d39-128">[Vytvoření větve kanálu](xref:fundamentals/middleware/index#middleware-run-map-use) umožňuje odesílat žádosti do konkrétní middleware, podle ne jenom adresu URL, ale i na hlavičky požadavku, řetězce dotazu, atd.</span><span class="sxs-lookup"><span data-stu-id="a5d39-128">[Pipeline branching](xref:fundamentals/middleware/index#middleware-run-map-use) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

<span data-ttu-id="a5d39-129">**Middleware jsou velmi podobné moduly:**</span><span class="sxs-lookup"><span data-stu-id="a5d39-129">**Middleware are very similar to modules:**</span></span>

   * <span data-ttu-id="a5d39-130">Volá v zásadě pro každý požadavek</span><span class="sxs-lookup"><span data-stu-id="a5d39-130">Invoked in principle for every request</span></span>

   * <span data-ttu-id="a5d39-131">Může krátká smyčka žádost, pomocí [není předání požadavku do dalšího middlewaru](#http-modules-shortcircuiting-middleware)</span><span class="sxs-lookup"><span data-stu-id="a5d39-131">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

   * <span data-ttu-id="a5d39-132">K vytvoření vlastních odpovědi HTTP</span><span class="sxs-lookup"><span data-stu-id="a5d39-132">Able to create their own HTTP response</span></span>

<span data-ttu-id="a5d39-133">**Middleware a moduly jsou zpracovány v jiném pořadí:**</span><span class="sxs-lookup"><span data-stu-id="a5d39-133">**Middleware and modules are processed in a different order:**</span></span>

   * <span data-ttu-id="a5d39-134">Pořadí middlewaru je založeno na pořadí, ve kterém jsou vkládána do kanálu požadavku, zatímco především podle pořadí modulů [životního cyklu aplikace](https://msdn.microsoft.com/library/ms227673.aspx) události</span><span class="sxs-lookup"><span data-stu-id="a5d39-134">Order of middleware is based on the order in which they're inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

   * <span data-ttu-id="a5d39-135">Pořadí middleware pro odpovědi je zpětného od pro požadavky, zatímco pořadí modulů je stejný pro požadavky a odpovědi</span><span class="sxs-lookup"><span data-stu-id="a5d39-135">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

   * <span data-ttu-id="a5d39-136">V tématu [vytváření middlewaru v řadě s IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="a5d39-136">See [Creating a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![Middleware](http-modules/_static/middleware.png)

<span data-ttu-id="a5d39-138">Všimněte si, jak na předchozím obrázku middleware ověřování short-circuited žádosti.</span><span class="sxs-lookup"><span data-stu-id="a5d39-138">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="a5d39-139">Migrace modulu kód pro middleware</span><span class="sxs-lookup"><span data-stu-id="a5d39-139">Migrating module code to middleware</span></span>

<span data-ttu-id="a5d39-140">Existující modul HTTP bude vypadat podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="a5d39-140">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="a5d39-141">Jak je znázorněno v [Middleware](xref:fundamentals/middleware/index) stránka, middlewaru ASP.NET Core je třída, která zveřejňuje `Invoke` metoda pořízení `HttpContext` a vrácení `Task`.</span><span class="sxs-lookup"><span data-stu-id="a5d39-141">As shown in the [Middleware](xref:fundamentals/middleware/index) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="a5d39-142">Vaše nové middleware bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="a5d39-142">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="a5d39-143">Předchozí middleware šablony pořízení z části [zápis middleware](xref:fundamentals/middleware/index#middleware-writing-middleware).</span><span class="sxs-lookup"><span data-stu-id="a5d39-143">The preceding middleware template was taken from the section on [writing middleware](xref:fundamentals/middleware/index#middleware-writing-middleware).</span></span>

<span data-ttu-id="a5d39-144">*MyMiddlewareExtensions* pomocná třída usnadňuje konfiguraci vlastního middlewaru v vaší `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="a5d39-144">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="a5d39-145">`UseMyMiddleware` Metoda přidá do kanálu žádost o třídě middleware.</span><span class="sxs-lookup"><span data-stu-id="a5d39-145">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="a5d39-146">V konstruktoru middlewaru získat vložit služeb požadovaných middleware.</span><span class="sxs-lookup"><span data-stu-id="a5d39-146">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="a5d39-147">Modul může ukončit žádost, například pokud uživatel nemá oprávnění:</span><span class="sxs-lookup"><span data-stu-id="a5d39-147">Your module might terminate a request, for example if the user isn't authorized:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="a5d39-148">Middleware zpracovává tento není voláním `Invoke` na další middleware v kanálu.</span><span class="sxs-lookup"><span data-stu-id="a5d39-148">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="a5d39-149">Mějte na paměti, že to není ukončit plně požadavek, protože předchozí middlewares bude stále vyvolán při odpovědi díky zpět skrze kanálu.</span><span class="sxs-lookup"><span data-stu-id="a5d39-149">Keep in mind that this doesn't fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="a5d39-150">Při migraci vaší modulu funkce do vaší nové middleware, můžete zjistit, že kód není kompilovat, protože `HttpContext` v ASP.NET Core výrazně změnila třída.</span><span class="sxs-lookup"><span data-stu-id="a5d39-150">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="a5d39-151">[Později na](#migrating-to-the-new-httpcontext), uvidíte, jak migrovat do nového HttpContext základní technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a5d39-151">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="a5d39-152">Migrace modulu vložení do kanálu požadavku</span><span class="sxs-lookup"><span data-stu-id="a5d39-152">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="a5d39-153">Vytváření modulů HTTP jsou obvykle přidány do žádosti o kanálu pomocí *Web.config*:</span><span class="sxs-lookup"><span data-stu-id="a5d39-153">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="a5d39-154">Převést tím, že [přidání vlastního nové middlewaru](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder) na kanále vaší `Startup` – třída:</span><span class="sxs-lookup"><span data-stu-id="a5d39-154">Convert this by [adding your new middleware](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="a5d39-155">Přesnou pozici v kanálu, kde vložit vaše nové middleware závisí na událost, která ji zpracovává jako modul (`BeginRequest`, `EndRequest`atd) a jeho pořadí v seznamu modulů v *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="a5d39-155">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="a5d39-156">Jako dříve jsme si říkali, neexistuje žádný životního cyklu aplikace v ASP.NET Core a pořadí, ve kterém jsou zpracovány odpovědí middlewarem se liší od pořadí použít moduly.</span><span class="sxs-lookup"><span data-stu-id="a5d39-156">As previously stated, there's no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="a5d39-157">To může se rozhodnout řazení další náročné.</span><span class="sxs-lookup"><span data-stu-id="a5d39-157">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="a5d39-158">Řazení přestane být problém, může rozdělit modul do více součástí middleware, které lze provést řazení nezávisle.</span><span class="sxs-lookup"><span data-stu-id="a5d39-158">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="a5d39-159">Migrace kód obslužné rutiny pro middleware</span><span class="sxs-lookup"><span data-stu-id="a5d39-159">Migrating handler code to middleware</span></span>

<span data-ttu-id="a5d39-160">Obslužné rutiny HTTP vypadá přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="a5d39-160">An HTTP handler looks something like this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="a5d39-161">V projektu ASP.NET Core by to nepřeloží na middleware podobná této:</span><span class="sxs-lookup"><span data-stu-id="a5d39-161">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="a5d39-162">Tento middleware je velmi podobný middleware odpovídající moduly.</span><span class="sxs-lookup"><span data-stu-id="a5d39-162">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="a5d39-163">Pouze skutečné rozdílem je, že zde není žádná volání `_next.Invoke(context)`.</span><span class="sxs-lookup"><span data-stu-id="a5d39-163">The only real difference is that here there's no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="a5d39-164">To dává smysl, protože rutina je na konci kanálu požadavku, takže neexistují žádné další middleware má být vyvolána.</span><span class="sxs-lookup"><span data-stu-id="a5d39-164">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="a5d39-165">Migrace obslužná rutina vložení do kanálu požadavku</span><span class="sxs-lookup"><span data-stu-id="a5d39-165">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="a5d39-166">Konfigurace obslužné rutiny HTTP se provádí v *Web.config* a vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="a5d39-166">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="a5d39-167">To může převést přidáním vlastního middlewaru novou obslužnou rutinu do kanálu požadavku ve vaší `Startup` třída, podobně jako middleware převést z modulů.</span><span class="sxs-lookup"><span data-stu-id="a5d39-167">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="a5d39-168">Problém s tohoto přístupu je, že ho do vaší nové middleware obslužnou rutinu by odesílat všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="a5d39-168">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="a5d39-169">Ale chcete jenom žádosti s příponou dané k dosažení vlastního middlewaru.</span><span class="sxs-lookup"><span data-stu-id="a5d39-169">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="a5d39-170">Který, získáte stejným funkcím, jaké jste měli s vaší obslužné rutiny HTTP.</span><span class="sxs-lookup"><span data-stu-id="a5d39-170">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="a5d39-171">Jedno řešení je větev kanálu pro požadavky s danou příponu, pomocí `MapWhen` metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="a5d39-171">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="a5d39-172">To uděláte ve stejné `Configure` metoda, kde můžete přidat další middleware:</span><span class="sxs-lookup"><span data-stu-id="a5d39-172">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="a5d39-173">`MapWhen`mají tyto parametry:</span><span class="sxs-lookup"><span data-stu-id="a5d39-173">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="a5d39-174">Lambda, která přebírá `HttpContext` a vrátí `true` Pokud požadavek by měl přejděte dolů větev.</span><span class="sxs-lookup"><span data-stu-id="a5d39-174">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="a5d39-175">To znamená, že můžete větev požadavky nejen založené na jejich rozšíření, ale také na hlavičky požadavku, parametrů řetězce dotazu, atd.</span><span class="sxs-lookup"><span data-stu-id="a5d39-175">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="a5d39-176">Lambda, který přebírá `IApplicationBuilder` a přidá všechny middleware pro větev.</span><span class="sxs-lookup"><span data-stu-id="a5d39-176">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="a5d39-177">To znamená, že můžete přidat další middleware do větve před vlastního middlewaru obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="a5d39-177">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="a5d39-178">Middleware přidán do kanálu, než větev, který bude vyvolán u všech požadavků; větev nebude mít vliv na ně.</span><span class="sxs-lookup"><span data-stu-id="a5d39-178">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="a5d39-179">Možnosti middlewaru pomocí vzoru možnosti načítání</span><span class="sxs-lookup"><span data-stu-id="a5d39-179">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="a5d39-180">Některé moduly a obslužné rutiny mají možnosti konfigurace, které jsou uložené v *Web.config*. V ASP.NET Core nový model konfigurace se ale používá místě *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="a5d39-180">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="a5d39-181">Nové [konfigurační systém](xref:fundamentals/configuration/index) vám dává tyto možnosti, chcete-li tento problém vyřešit:</span><span class="sxs-lookup"><span data-stu-id="a5d39-181">The new [configuration system](xref:fundamentals/configuration/index) gives you these options to solve this:</span></span>

* <span data-ttu-id="a5d39-182">Přímo vložit možnosti do middleware, jak je znázorněno [další části](#loading-middleware-options-through-direct-injection).</span><span class="sxs-lookup"><span data-stu-id="a5d39-182">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="a5d39-183">Použití [možnosti vzor](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="a5d39-183">Use the [options pattern](xref:fundamentals/configuration/options):</span></span>

1.  <span data-ttu-id="a5d39-184">Vytvoření třídy k umístění middleware možnosti, například:</span><span class="sxs-lookup"><span data-stu-id="a5d39-184">Create a class to hold your middleware options, for example:</span></span>

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  <span data-ttu-id="a5d39-185">Uložení možnost hodnot</span><span class="sxs-lookup"><span data-stu-id="a5d39-185">Store the option values</span></span>

    <span data-ttu-id="a5d39-186">Konfigurace systému umožňuje ukládat hodnoty možnosti kdekoli chcete.</span><span class="sxs-lookup"><span data-stu-id="a5d39-186">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="a5d39-187">Ale většina lokality použijte *appSettings.JSON určený*, takže provedeme tohoto přístupu:</span><span class="sxs-lookup"><span data-stu-id="a5d39-187">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    <span data-ttu-id="a5d39-188">*MyMiddlewareOptionsSection* následuje název oddílu.</span><span class="sxs-lookup"><span data-stu-id="a5d39-188">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="a5d39-189">Nemusí to být stejný jako název třídy vaše možnosti.</span><span class="sxs-lookup"><span data-stu-id="a5d39-189">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="a5d39-190">Hodnoty možnosti přidružit možnosti – třída</span><span class="sxs-lookup"><span data-stu-id="a5d39-190">Associate the option values with the options class</span></span>

    <span data-ttu-id="a5d39-191">Vzor možnosti používá framework vkládání závislostí ASP.NET Core k přidružení typu možnosti (například `MyMiddlewareOptions`) s `MyMiddlewareOptions` objekt, který obsahuje skutečné možnosti.</span><span class="sxs-lookup"><span data-stu-id="a5d39-191">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="a5d39-192">Aktualizace vašeho `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="a5d39-192">Update your `Startup` class:</span></span>

    1.  <span data-ttu-id="a5d39-193">Pokud používáte *appSettings.JSON určený*, přidejte ho do Tvůrce konfigurace v `Startup` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="a5d39-193">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  <span data-ttu-id="a5d39-194">Konfigurovat službu možnosti:</span><span class="sxs-lookup"><span data-stu-id="a5d39-194">Configure the options service:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  <span data-ttu-id="a5d39-195">Možnosti přidružte třídě možnosti:</span><span class="sxs-lookup"><span data-stu-id="a5d39-195">Associate your options with your options class:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  <span data-ttu-id="a5d39-196">Vložit možnosti do vaší konstruktor middlewaru.</span><span class="sxs-lookup"><span data-stu-id="a5d39-196">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="a5d39-197">Toto je podobná vložení možnosti do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="a5d39-197">This is similar to injecting options into a controller.</span></span>

  [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  <span data-ttu-id="a5d39-198">[UseMiddleware](#http-modules-usemiddleware) metody rozšíření, která přidá vaše middlewaru, který má `IApplicationBuilder` postará vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="a5d39-198">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

  <span data-ttu-id="a5d39-199">Tato akce není omezen na `IOptions` objekty.</span><span class="sxs-lookup"><span data-stu-id="a5d39-199">This isn't limited to `IOptions` objects.</span></span> <span data-ttu-id="a5d39-200">Tímto způsobem může vložit jakýkoliv jiný objekt, který vyžaduje vlastního middlewaru.</span><span class="sxs-lookup"><span data-stu-id="a5d39-200">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="a5d39-201">Možnosti middlewaru prostřednictvím přímé vkládání načítání</span><span class="sxs-lookup"><span data-stu-id="a5d39-201">Loading middleware options through direct injection</span></span>

<span data-ttu-id="a5d39-202">Vzor možnosti má výhodu, který vytvoří přijít spojení mezi hodnotami možnosti a jejich příjemce.</span><span class="sxs-lookup"><span data-stu-id="a5d39-202">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="a5d39-203">Po třída možností jste přidružené hodnoty skutečné možnosti, jiná třída lze získat přístup k možnostem prostřednictvím rozhraní vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="a5d39-203">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="a5d39-204">Není nutné předat kolem hodnoty možnosti.</span><span class="sxs-lookup"><span data-stu-id="a5d39-204">There's no need to pass around options values.</span></span>

<span data-ttu-id="a5d39-205">To rozpis ale pokud chcete použít stejné middleware dvakrát, s různými možnostmi.</span><span class="sxs-lookup"><span data-stu-id="a5d39-205">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="a5d39-206">Například autorizaci middleware použitý v různých pobočkách, která umožňuje různé role.</span><span class="sxs-lookup"><span data-stu-id="a5d39-206">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="a5d39-207">Pomocí třídy jeden možnosti nelze přidružit dva objekty různé možnosti.</span><span class="sxs-lookup"><span data-stu-id="a5d39-207">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="a5d39-208">Řešení je získat možnosti objekty s hodnotami skutečné možnosti ve vaší `Startup` třídy a předejte těch, které přímo ke každé instanci vlastního middlewaru.</span><span class="sxs-lookup"><span data-stu-id="a5d39-208">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1.  <span data-ttu-id="a5d39-209">Přidat druhý klíč do *appSettings.JSON určený*</span><span class="sxs-lookup"><span data-stu-id="a5d39-209">Add a second key to *appsettings.json*</span></span>

    <span data-ttu-id="a5d39-210">Chcete-li přidat druhý sadu možností a *appSettings.JSON určený* soubor, použijte nový klíč k jeho jednoznačné identifikaci:</span><span class="sxs-lookup"><span data-stu-id="a5d39-210">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  <span data-ttu-id="a5d39-211">Načíst hodnoty možností a předejte middleware.</span><span class="sxs-lookup"><span data-stu-id="a5d39-211">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="a5d39-212">`Use...` – Metoda rozšíření (která vlastního middlewaru přidá do kanálu) je logické místo, kde můžete předat hodnoty možnosti:</span><span class="sxs-lookup"><span data-stu-id="a5d39-212">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  <span data-ttu-id="a5d39-213">Povolí middleware má provést parametrem možnosti.</span><span class="sxs-lookup"><span data-stu-id="a5d39-213">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="a5d39-214">Zadejte přetížení `Use...` metoda rozšíření (která přebírá parametr možnosti a předává jej do `UseMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="a5d39-214">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="a5d39-215">Když `UseMiddleware` je volána s parametry, předává parametry do vaší konstruktor middlewaru při vytvoření instance objektu middleware.</span><span class="sxs-lookup"><span data-stu-id="a5d39-215">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

    [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    <span data-ttu-id="a5d39-216">Všimněte si, jak to zabalí objekt možnosti v `OptionsWrapper` objektu.</span><span class="sxs-lookup"><span data-stu-id="a5d39-216">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="a5d39-217">To implementuje `IOptions`, podle očekávání tím konstruktor middlewaru.</span><span class="sxs-lookup"><span data-stu-id="a5d39-217">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="a5d39-218">Migrace na nové HttpContext</span><span class="sxs-lookup"><span data-stu-id="a5d39-218">Migrating to the new HttpContext</span></span>

<span data-ttu-id="a5d39-219">Už jste viděli dříve, `Invoke` metoda v vlastního middlewaru přebírá parametr typu `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="a5d39-219">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="a5d39-220">`HttpContext`výrazně se změnilo v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a5d39-220">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="a5d39-221">V této části ukazuje, jak převede nejčastěji používané vlastnosti [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) do nového `Microsoft.AspNetCore.Http.HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="a5d39-221">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="a5d39-222">HttpContext</span><span class="sxs-lookup"><span data-stu-id="a5d39-222">HttpContext</span></span>

<span data-ttu-id="a5d39-223">**HttpContext.Items** přeloží na:</span><span class="sxs-lookup"><span data-stu-id="a5d39-223">**HttpContext.Items** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="a5d39-224">**ID jedinečné žádosti (System.Web.HttpContext protějšek)**</span><span class="sxs-lookup"><span data-stu-id="a5d39-224">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="a5d39-225">Poskytuje jedinečné id pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="a5d39-225">Gives you a unique id for each request.</span></span> <span data-ttu-id="a5d39-226">Velmi užitečné zahrnout do protokolů.</span><span class="sxs-lookup"><span data-stu-id="a5d39-226">Very useful to include in your logs.</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="a5d39-227">HttpContext.Request</span><span class="sxs-lookup"><span data-stu-id="a5d39-227">HttpContext.Request</span></span>

<span data-ttu-id="a5d39-228">**HttpContext.Request.HttpMethod** přeloží na:</span><span class="sxs-lookup"><span data-stu-id="a5d39-228">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="a5d39-229">**HttpContext.Request.QueryString** translates to:</span><span class="sxs-lookup"><span data-stu-id="a5d39-229">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="a5d39-230">**HttpContext.Request.Url** a **HttpContext.Request.RawUrl** nepřeloží na:</span><span class="sxs-lookup"><span data-stu-id="a5d39-230">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="a5d39-231">**HttpContext.Request.IsSecureConnection** translates to:</span><span class="sxs-lookup"><span data-stu-id="a5d39-231">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="a5d39-232">**HttpContext.Request.UserHostAddress** translates to:</span><span class="sxs-lookup"><span data-stu-id="a5d39-232">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="a5d39-233">**HttpContext.Request.Cookies** přeloží na:</span><span class="sxs-lookup"><span data-stu-id="a5d39-233">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="a5d39-234">**HttpContext.Request.RequestContext.RouteData** translates to:</span><span class="sxs-lookup"><span data-stu-id="a5d39-234">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="a5d39-235">**HttpContext.Request.Headers** translates to:</span><span class="sxs-lookup"><span data-stu-id="a5d39-235">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="a5d39-236">**HttpContext.Request.UserAgent** translates to:</span><span class="sxs-lookup"><span data-stu-id="a5d39-236">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="a5d39-237">**HttpContext.Request.UrlReferrer** translates to:</span><span class="sxs-lookup"><span data-stu-id="a5d39-237">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="a5d39-238">**HttpContext.Request.ContentType** translates to:</span><span class="sxs-lookup"><span data-stu-id="a5d39-238">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="a5d39-239">**HttpContext.Request.Form** přeloží na:</span><span class="sxs-lookup"><span data-stu-id="a5d39-239">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="a5d39-240">Čtení hodnot formuláře, pouze v případě, že je typ obsahu dílčí *x--www-form-urlencoded* nebo *data formuláře*.</span><span class="sxs-lookup"><span data-stu-id="a5d39-240">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="a5d39-241">**HttpContext.Request.InputStream** translates to:</span><span class="sxs-lookup"><span data-stu-id="a5d39-241">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="a5d39-242">Tento kód používejte pouze v middleware typ obslužné rutiny, na konci kanálu.</span><span class="sxs-lookup"><span data-stu-id="a5d39-242">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="a5d39-243">Nezpracovaná text si můžete přečíst jako v příkladu nahoře jen jednou na základě požadavku.</span><span class="sxs-lookup"><span data-stu-id="a5d39-243">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="a5d39-244">Při pokusu o čtení textu po první čtení middleware budou číst prázdným textem zprávy.</span><span class="sxs-lookup"><span data-stu-id="a5d39-244">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="a5d39-245">To neplatí pro čtení formuláře, jako je uvedené výše, vzhledem k tomu, které se provádí z vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="a5d39-245">This doesn't apply to reading a form as shown earlier, because that's done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="a5d39-246">HttpContext.Response</span><span class="sxs-lookup"><span data-stu-id="a5d39-246">HttpContext.Response</span></span>

<span data-ttu-id="a5d39-247">**HttpContext.Response.Status** a **HttpContext.Response.StatusDescription** nepřeloží na:</span><span class="sxs-lookup"><span data-stu-id="a5d39-247">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="a5d39-248">**HttpContext.Response.ContentEncoding** a **HttpContext.Response.ContentType** nepřeloží na:</span><span class="sxs-lookup"><span data-stu-id="a5d39-248">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="a5d39-249">**HttpContext.Response.ContentType** na svůj vlastní také překládá do:</span><span class="sxs-lookup"><span data-stu-id="a5d39-249">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="a5d39-250">**HttpContext.Response.Output** přeloží na:</span><span class="sxs-lookup"><span data-stu-id="a5d39-250">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="a5d39-251">**HttpContext.Response.TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="a5d39-251">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="a5d39-252">Obsluhující si soubor popsané [zde](../fundamentals/request-features.md#middleware-and-request-features).</span><span class="sxs-lookup"><span data-stu-id="a5d39-252">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="a5d39-253">**HttpContext.Response.Headers**</span><span class="sxs-lookup"><span data-stu-id="a5d39-253">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="a5d39-254">Odeslání hlaviček odpovědí ztěžuje skutečnost, že pokud jste nastavili, je po nic byla zapsána na text odpovědi, se nebude odeslána.</span><span class="sxs-lookup"><span data-stu-id="a5d39-254">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="a5d39-255">Řešení je nastavena metoda zpětného volání, která bude volána vpravo před zápisem na spustí odpověď.</span><span class="sxs-lookup"><span data-stu-id="a5d39-255">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="a5d39-256">To se nejlépe provádí na začátku `Invoke` metoda v vlastního middlewaru.</span><span class="sxs-lookup"><span data-stu-id="a5d39-256">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="a5d39-257">Je tato metoda zpětného volání, která nastavuje vaše hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a5d39-257">It's this callback method that sets your response headers.</span></span>

<span data-ttu-id="a5d39-258">Následující kód nastaví zpětné volání metodu s názvem `SetHeaders`:</span><span class="sxs-lookup"><span data-stu-id="a5d39-258">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="a5d39-259">`SetHeaders` Metoda zpětného volání bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="a5d39-259">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="a5d39-260">**HttpContext.Response.Cookies**</span><span class="sxs-lookup"><span data-stu-id="a5d39-260">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="a5d39-261">Soubory cookie dostavit do prohlížeče v *Set-Cookie* hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a5d39-261">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="a5d39-262">V důsledku toho odesílání souborů cookie vyžaduje stejné zpětného volání jako používané pro odeslání hlaviček odpovědí:</span><span class="sxs-lookup"><span data-stu-id="a5d39-262">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="a5d39-263">`SetCookies` Metoda zpětného volání by vypadat třeba takto:</span><span class="sxs-lookup"><span data-stu-id="a5d39-263">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="a5d39-264">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a5d39-264">Additional resources</span></span>

* [<span data-ttu-id="a5d39-265">Přehled modulů HTTP a obslužné rutiny HTTP</span><span class="sxs-lookup"><span data-stu-id="a5d39-265">HTTP Handlers and HTTP Modules Overview</span></span>](/iis/configuration/system.webserver/)
* [<span data-ttu-id="a5d39-266">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="a5d39-266">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="a5d39-267">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="a5d39-267">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="a5d39-268">Middleware</span><span class="sxs-lookup"><span data-stu-id="a5d39-268">Middleware</span></span>](xref:fundamentals/middleware/index)
