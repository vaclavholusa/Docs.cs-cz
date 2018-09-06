---
title: Povolení žádostí napříč zdroji (CORS) v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak CORS jako standard pro povolení nebo odmítnutí žádostí nepůvodního v aplikaci ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2018
uid: security/cors
ms.openlocfilehash: f654260411f1bd5725a0e3d14951c7e9bbc893e8
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/06/2018
ms.locfileid: "44039975"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="95b05-103">Povolení žádostí napříč zdroji (CORS) v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="95b05-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="95b05-104">Podle [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), a [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="95b05-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="95b05-105">Zabezpečení prohlížečů brání zasílání požadavků na jiné doméně než ten, který obsluhuje webovou stránku na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="95b05-105">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="95b05-106">Toto omezení je volána *zásada stejného zdroje*.</span><span class="sxs-lookup"><span data-stu-id="95b05-106">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="95b05-107">Zásada stejného zdroje brání škodlivým webům ve čtení citlivých dat z jiné lokality.</span><span class="sxs-lookup"><span data-stu-id="95b05-107">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="95b05-108">V některých případech můžete chtít povolit, že ostatní lokality provádět požadavky cross-origin do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="95b05-108">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span>

<span data-ttu-id="95b05-109">[Mezi sdílení zdrojů původu](https://www.w3.org/TR/cors/) (CORS) je standard W3C, která umožňuje server zmírnit zásadu stejného zdroje.</span><span class="sxs-lookup"><span data-stu-id="95b05-109">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="95b05-110">Pomocí CORS, server explicitně můžou některé požadavky cross-origin zatímco jiné odmítnout.</span><span class="sxs-lookup"><span data-stu-id="95b05-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="95b05-111">CORS je bezpečnější a flexibilnější, než starší techniky, jako například [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="95b05-111">CORS is safer and more flexible than earlier techniques, such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="95b05-112">Toto téma ukazuje, jak povolit CORS v aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="95b05-112">This topic shows how to enable CORS in an ASP.NET Core app.</span></span>

## <a name="same-origin"></a><span data-ttu-id="95b05-113">Stejného původu</span><span class="sxs-lookup"><span data-stu-id="95b05-113">Same origin</span></span>

<span data-ttu-id="95b05-114">Pokud mají stejné schémata, hostitele a porty mít dvě adresy URL stejného původu ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="95b05-114">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="95b05-115">Tyto dvě adresy URL mají stejného původu:</span><span class="sxs-lookup"><span data-stu-id="95b05-115">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="95b05-116">Tyto adresy URL mají různé zdroje než předchozí dvě adresy URL:</span><span class="sxs-lookup"><span data-stu-id="95b05-116">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="95b05-117">`https://example.net` &ndash; Jiné domény</span><span class="sxs-lookup"><span data-stu-id="95b05-117">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="95b05-118">`https://www.example.com/foo.html` &ndash; Různé subdomény</span><span class="sxs-lookup"><span data-stu-id="95b05-118">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="95b05-119">`http://example.com/foo.html` &ndash; Jiné schéma</span><span class="sxs-lookup"><span data-stu-id="95b05-119">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="95b05-120">`https://example.com:9000/foo.html` &ndash; Jiný port</span><span class="sxs-lookup"><span data-stu-id="95b05-120">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

> [!NOTE]
> <span data-ttu-id="95b05-121">Aplikace Internet Explorer nezahrne port při porovnání zdrojů.</span><span class="sxs-lookup"><span data-stu-id="95b05-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="register-cors-services"></a><span data-ttu-id="95b05-122">Registrace služby CORS</span><span class="sxs-lookup"><span data-stu-id="95b05-122">Register CORS services</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="95b05-123">Odkaz [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app) nebo přidat odkaz na balíček [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="95b05-123">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="95b05-124">Odkaz [metabalíček Microsoft.aspnetcore.all](xref:fundamentals/metapackage) nebo přidat odkaz na balíček [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="95b05-124">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="95b05-125">Přidejte odkaz na balíček [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="95b05-125">Add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

<span data-ttu-id="95b05-126">Volání <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> v `Startup.ConfigureServices` přidejte CORS služby do aplikace služby kontejneru:</span><span class="sxs-lookup"><span data-stu-id="95b05-126">Call <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` to add CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a><span data-ttu-id="95b05-127">Povolení CORS</span><span class="sxs-lookup"><span data-stu-id="95b05-127">Enable CORS</span></span>

<span data-ttu-id="95b05-128">Po registraci služby CORS, povolení CORS v aplikaci ASP.NET Core pomocí některé z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="95b05-128">After registering CORS services, use either of the following approaches to enable CORS in an ASP.NET Core app:</span></span>

* <span data-ttu-id="95b05-129">[CORS Middleware](#enable-cors-with-cors-middleware) &ndash; zásady CORS platí globálně do aplikace přes middleware.</span><span class="sxs-lookup"><span data-stu-id="95b05-129">[CORS Middleware](#enable-cors-with-cors-middleware) &ndash; Apply CORS policies globally to the app via middleware.</span></span>
* <span data-ttu-id="95b05-130">[CORS v MVC](#enable-cors-in-mvc) &ndash; CORS použít zásady podle akce nebo kontroleru.</span><span class="sxs-lookup"><span data-stu-id="95b05-130">[CORS in MVC](#enable-cors-in-mvc) &ndash; Apply CORS policies per action or per controller.</span></span> <span data-ttu-id="95b05-131">CORS Middleware se nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="95b05-131">CORS Middleware isn't used.</span></span>

### <a name="enable-cors-with-cors-middleware"></a><span data-ttu-id="95b05-132">Povolení CORS s Middlewarem CORS</span><span class="sxs-lookup"><span data-stu-id="95b05-132">Enable CORS with CORS Middleware</span></span>

<span data-ttu-id="95b05-133">CORS Middleware zpracovává požadavky cross-origin do aplikace.</span><span class="sxs-lookup"><span data-stu-id="95b05-133">CORS Middleware handles cross-origin requests to the app.</span></span> <span data-ttu-id="95b05-134">Chcete-li povolit CORS Middleware v kanálu zpracování požadavků, zavolejte <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> metody rozšíření v `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="95b05-134">To enable CORS Middleware in the request processing pipeline, call the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method in `Startup.Configure`.</span></span>

<span data-ttu-id="95b05-135">CORS Middleware musí předcházet všechny koncové body definované ve vaší aplikaci ve které chcete podporovat požadavky cross-origin (například před voláním `UseMvc` pro Middleware stránky MVC a Razor).</span><span class="sxs-lookup"><span data-stu-id="95b05-135">CORS Middleware must precede any defined endpoints in your app where you want to support cross-origin requests (for example, before the call to `UseMvc` for MVC/Razor Pages Middleware).</span></span>

<span data-ttu-id="95b05-136">A *nepůvodního zdroje zásad* lze zadat při přidání pomocí CORS Middleware <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> třídy.</span><span class="sxs-lookup"><span data-stu-id="95b05-136">A *cross-origin policy* can be specified when adding the CORS Middleware using the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> class.</span></span> <span data-ttu-id="95b05-137">Existují dva přístupy k definování zásad CORS:</span><span class="sxs-lookup"><span data-stu-id="95b05-137">There are two approaches for defining a CORS policy:</span></span>

* <span data-ttu-id="95b05-138">Volání `UseCors` pomocí výrazu lambda:</span><span class="sxs-lookup"><span data-stu-id="95b05-138">Call `UseCors` with a lambda:</span></span>

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  <span data-ttu-id="95b05-139">Používá výraz lambda <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> objektu.</span><span class="sxs-lookup"><span data-stu-id="95b05-139">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="95b05-140">[Možnosti konfigurace](#cors-policy-options), jako například `WithOrigins`, jsou popsány dále v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="95b05-140">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this topic.</span></span> <span data-ttu-id="95b05-141">V předchozím příkladu, zásada umožňuje požadavky cross-origin z `https://example.com` a žádné jiné zdroje.</span><span class="sxs-lookup"><span data-stu-id="95b05-141">In the preceding example, the policy allows cross-origin requests from `https://example.com` and no other origins.</span></span>

  <span data-ttu-id="95b05-142">Adresa URL musí být zadán bez koncové lomítko (`/`).</span><span class="sxs-lookup"><span data-stu-id="95b05-142">The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="95b05-143">Pokud adresa URL se ukončí s `/`, porovnání vrátí `false` a vrátí se bez záhlaví.</span><span class="sxs-lookup"><span data-stu-id="95b05-143">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

  <span data-ttu-id="95b05-144">`CorsPolicyBuilder` obsahuje rozhraní API fluent, takže můžete vytvořit řetězové volání metody:</span><span class="sxs-lookup"><span data-stu-id="95b05-144">`CorsPolicyBuilder` has a fluent API, so you can chain method calls:</span></span>

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* <span data-ttu-id="95b05-145">Definovat jeden nebo více s názvem zásady CORS a vyberte zásadu podle názvu za běhu.</span><span class="sxs-lookup"><span data-stu-id="95b05-145">Define one or more named CORS policies and select the policy by name at runtime.</span></span> <span data-ttu-id="95b05-146">Následující příklad přidá uživatelem definované zásady CORS s názvem *AllowSpecificOrigin*.</span><span class="sxs-lookup"><span data-stu-id="95b05-146">The following example adds a user-defined CORS policy named *AllowSpecificOrigin*.</span></span> <span data-ttu-id="95b05-147">Vyberte zásadu, předejte název, který má `UseCors`:</span><span class="sxs-lookup"><span data-stu-id="95b05-147">To select the policy, pass the name to `UseCors`:</span></span>

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a><span data-ttu-id="95b05-148">Povolení CORS v aplikaci MVC</span><span class="sxs-lookup"><span data-stu-id="95b05-148">Enable CORS in MVC</span></span>

<span data-ttu-id="95b05-149">Můžete také použít MVC použít konkrétní zásady CORS podle akce nebo kontroleru.</span><span class="sxs-lookup"><span data-stu-id="95b05-149">You can alternatively use MVC to apply specific CORS policies per action or per controller.</span></span> <span data-ttu-id="95b05-150">Při povolení CORS pomocí MVC, se používají registrovaných služeb CORS.</span><span class="sxs-lookup"><span data-stu-id="95b05-150">When using MVC to enable CORS, the registered CORS services are used.</span></span> <span data-ttu-id="95b05-151">CORS Middleware se nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="95b05-151">The CORS Middleware isn't used.</span></span>

### <a name="per-action"></a><span data-ttu-id="95b05-152">Za akci</span><span class="sxs-lookup"><span data-stu-id="95b05-152">Per action</span></span>

<span data-ttu-id="95b05-153">Chcete-li určit zásady CORS pro určité akce, přidejte [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atribut na akci.</span><span class="sxs-lookup"><span data-stu-id="95b05-153">To specify a CORS policy for a specific action, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the action.</span></span> <span data-ttu-id="95b05-154">Zadejte název zásady.</span><span class="sxs-lookup"><span data-stu-id="95b05-154">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a><span data-ttu-id="95b05-155">Na kontroler</span><span class="sxs-lookup"><span data-stu-id="95b05-155">Per controller</span></span>

<span data-ttu-id="95b05-156">Chcete-li určit zásady CORS pro konkrétní ovladač, přidejte [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atribut třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="95b05-156">To specify the CORS policy for a specific controller, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the controller class.</span></span> <span data-ttu-id="95b05-157">Zadejte název zásady.</span><span class="sxs-lookup"><span data-stu-id="95b05-157">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

<span data-ttu-id="95b05-158">Pořadí priority je:</span><span class="sxs-lookup"><span data-stu-id="95b05-158">The precedence order is:</span></span>

1. <span data-ttu-id="95b05-159">Akce</span><span class="sxs-lookup"><span data-stu-id="95b05-159">action</span></span>
1. <span data-ttu-id="95b05-160">kontroler</span><span class="sxs-lookup"><span data-stu-id="95b05-160">controller</span></span>

### <a name="disable-cors"></a><span data-ttu-id="95b05-161">Zákazu sdílení CORS</span><span class="sxs-lookup"><span data-stu-id="95b05-161">Disable CORS</span></span>

<span data-ttu-id="95b05-162">Chcete-li zakázat CORS pro kontroler nebo akce, použijte [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) atribut:</span><span class="sxs-lookup"><span data-stu-id="95b05-162">To disable CORS for a controller or action, use the [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a><span data-ttu-id="95b05-163">Možnosti zásad CORS</span><span class="sxs-lookup"><span data-stu-id="95b05-163">CORS policy options</span></span>

<span data-ttu-id="95b05-164">Tato část popisuje různé možnosti, které můžete nastavit zásadu CORS.</span><span class="sxs-lookup"><span data-stu-id="95b05-164">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="95b05-165">Nastavte povolené zdroje</span><span class="sxs-lookup"><span data-stu-id="95b05-165">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="95b05-166">Nastavte povolené metody HTTP</span><span class="sxs-lookup"><span data-stu-id="95b05-166">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="95b05-167">Nastavit hlavičku povolené žádosti</span><span class="sxs-lookup"><span data-stu-id="95b05-167">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="95b05-168">Nastavit hlavičky vystavené odpovědi</span><span class="sxs-lookup"><span data-stu-id="95b05-168">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="95b05-169">Přihlašovací údaje v požadavky cross-origin</span><span class="sxs-lookup"><span data-stu-id="95b05-169">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="95b05-170">Nastavit čas vypršení platnosti předběžné</span><span class="sxs-lookup"><span data-stu-id="95b05-170">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="95b05-171">Pro některé možnosti, může být užitečné ke čtení [funguje jak CORS](#how-cors-works) první části.</span><span class="sxs-lookup"><span data-stu-id="95b05-171">For some options, it may be helpful to read the [How CORS works](#how-cors-works) section first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="95b05-172">Nastavte povolené zdroje</span><span class="sxs-lookup"><span data-stu-id="95b05-172">Set the allowed origins</span></span>

<span data-ttu-id="95b05-173">Chcete-li povolit jeden nebo více konkrétních zdrojů, zavolejte <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>:</span><span class="sxs-lookup"><span data-stu-id="95b05-173">To allow one or more specific origins, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

<span data-ttu-id="95b05-174">Chcete-li povolit všechny původy, zavolejte <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>:</span><span class="sxs-lookup"><span data-stu-id="95b05-174">To allow all origins, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

<span data-ttu-id="95b05-175">Pečlivě zvažte předtím, než žádosti z původu.</span><span class="sxs-lookup"><span data-stu-id="95b05-175">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="95b05-176">Povolení žádostí z jakýkoli původ znamená, že *všechny weby* můžete provádět požadavky cross-origin vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="95b05-176">Allowing requests from any origin means that *any website* can make cross-origin requests to your app.</span></span>

<span data-ttu-id="95b05-177">Toto nastavení má vliv [časový limit předběžné požadavky a hlavičky Access-Control-Allow-Origin](#preflight-requests) (popsáno dále v tomto tématu).</span><span class="sxs-lookup"><span data-stu-id="95b05-177">This setting affects [preflight requests and the Access-Control-Allow-Origin header](#preflight-requests) (described later in this topic).</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="95b05-178">Nastavte povolené metody HTTP</span><span class="sxs-lookup"><span data-stu-id="95b05-178">Set the allowed HTTP methods</span></span>

<span data-ttu-id="95b05-179">Chcete-li povolit všechny metody HTTP, zavolejte <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="95b05-179">To allow all HTTP methods, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

<span data-ttu-id="95b05-180">Toto nastavení má vliv [časový limit předběžné požadavky a záhlaví přístupu – ovládací prvek-Allow-Methods](#preflight-requests) (popsáno dále v tomto tématu).</span><span class="sxs-lookup"><span data-stu-id="95b05-180">This setting affects [preflight requests and the Access-Control-Allow-Methods header](#preflight-requests) (described later in this topic).</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="95b05-181">Nastavit hlavičku povolené žádosti</span><span class="sxs-lookup"><span data-stu-id="95b05-181">Set the allowed request headers</span></span>

<span data-ttu-id="95b05-182">Volá se, aby konkrétní hlavičky se odešle žádost CORS *vytvářet hlavičky žádosti*, volání <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> a zadat povolené hlavičky:</span><span class="sxs-lookup"><span data-stu-id="95b05-182">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="95b05-183">Chcete-li povolit všechny hlavičky žádosti, vytvářet volání <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="95b05-183">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="95b05-184">Toto nastavení má vliv [časový limit předběžné požadavky a hlavičky Access-Control-Request-Headers](#preflight-requests) (popsáno dále v tomto tématu).</span><span class="sxs-lookup"><span data-stu-id="95b05-184">This setting affects [preflight requests and the Access-Control-Request-Headers header](#preflight-requests) (described later in this topic).</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="95b05-185">Shoda se zásadami Middlewarem CORS pro konkrétní hlavičky určené `WithHeaders` je možné, pouze při odeslání hlaviček `Access-Control-Request-Headers` přesně odpovídat záhlaví uvádí `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="95b05-185">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="95b05-186">Zvažte například aplikaci nakonfigurovány takto:</span><span class="sxs-lookup"><span data-stu-id="95b05-186">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="95b05-187">CORS Middleware předběžný požadavek s následující hlavičky žádosti odmítne, protože `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) není uveden ve `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="95b05-187">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="95b05-188">Vrátí aplikaci *200 OK* odpověď, ale nebude odesílat hlavičky CORS zpět.</span><span class="sxs-lookup"><span data-stu-id="95b05-188">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="95b05-189">Prohlížeč proto nebude se pokoušet žádosti nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="95b05-189">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="95b05-190">CORS Middleware vždy povoluje čtyři záhlaví v `Access-Control-Request-Headers` k odeslání bez ohledu na nakonfigurované v CorsPolicy.Headers hodnoty.</span><span class="sxs-lookup"><span data-stu-id="95b05-190">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="95b05-191">Tento seznam hlaviček obsahuje:</span><span class="sxs-lookup"><span data-stu-id="95b05-191">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="95b05-192">Zvažte například aplikaci nakonfigurovány takto:</span><span class="sxs-lookup"><span data-stu-id="95b05-192">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="95b05-193">CORS Middleware se úspěšně odpoví na předběžný požadavek s následující hlavičky žádosti protože `Content-Language` je vždy přidat na seznam povolených:</span><span class="sxs-lookup"><span data-stu-id="95b05-193">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="95b05-194">Nastavit hlavičky vystavené odpovědi</span><span class="sxs-lookup"><span data-stu-id="95b05-194">Set the exposed response headers</span></span>

<span data-ttu-id="95b05-195">Ve výchozím prohlížeči nezveřejňuje všechny hlavičky odpovědí do aplikace.</span><span class="sxs-lookup"><span data-stu-id="95b05-195">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="95b05-196">Další informace najdete v tématu [W3C napříč sdílení prostředků různého původu (terminologie): jednoduché hlavička odpovědi](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="95b05-196">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="95b05-197">Hlavičky odpovědi, které jsou k dispozici ve výchozím nastavení jsou:</span><span class="sxs-lookup"><span data-stu-id="95b05-197">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="95b05-198">Specifikace CORS volá tyto hlavičky *hlavičky odpovědi jednoduché*.</span><span class="sxs-lookup"><span data-stu-id="95b05-198">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="95b05-199">Chcete-li zpřístupnit další hlavičky pro aplikace, zavolejte <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="95b05-199">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="95b05-200">Přihlašovací údaje v požadavky cross-origin</span><span class="sxs-lookup"><span data-stu-id="95b05-200">Credentials in cross-origin requests</span></span>

<span data-ttu-id="95b05-201">Přihlašovací údaje vyžadují speciální zacházení v požadavku CORS.</span><span class="sxs-lookup"><span data-stu-id="95b05-201">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="95b05-202">Ve výchozím nastavení nebude prohlížeč odesílá pověření s žádostí nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="95b05-202">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="95b05-203">Přihlašovací údaje zahrnují soubory cookie a schémat ověřování protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="95b05-203">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="95b05-204">Odesílá pověření s žádostí nepůvodního zdroje, musíte nastavit klienta `XMLHttpRequest.withCredentials` k `true`.</span><span class="sxs-lookup"><span data-stu-id="95b05-204">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="95b05-205">Pomocí `XMLHttpRequest` přímo:</span><span class="sxs-lookup"><span data-stu-id="95b05-205">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="95b05-206">V jQuery:</span><span class="sxs-lookup"><span data-stu-id="95b05-206">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="95b05-207">Na serveru navíc musíte povolit přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="95b05-207">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="95b05-208">Chcete-li povolit přihlašovací údaje mezi zdroji, zavolejte <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="95b05-208">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

<span data-ttu-id="95b05-209">Odpověď HTTP, která zahrnuje `Access-Control-Allow-Credentials` hlavičky, která sděluje prohlížeči, že server umožňuje přihlašovací údaje pro žádosti nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="95b05-209">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="95b05-210">Pokud prohlížeč odesílá pověření, ale odpověď neobsahuje platný `Access-Control-Allow-Credentials` záhlaví, v prohlížeči nezveřejňuje odpovědi do aplikace a nepůvodního požadavek selže.</span><span class="sxs-lookup"><span data-stu-id="95b05-210">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="95b05-211">Buďte opatrní při povolování pověření nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="95b05-211">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="95b05-212">Web v jiné doméně odeslat do aplikace jménem uživatele bez vědomí uživatele pověření přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="95b05-212">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span>

<span data-ttu-id="95b05-213">Specifikace CORS také uvádí nastavení počátky k `"*"` (všechny původy) je neplatná-li `Access-Control-Allow-Credentials` záhlaví je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="95b05-213">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="95b05-214">Předběžných požadavků</span><span class="sxs-lookup"><span data-stu-id="95b05-214">Preflight requests</span></span>

<span data-ttu-id="95b05-215">U některých požadavků CORS prohlížeč odešle požadavek další před provedením aktuálního požadavku.</span><span class="sxs-lookup"><span data-stu-id="95b05-215">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="95b05-216">Tento požadavek je volána *předběžný požadavek*.</span><span class="sxs-lookup"><span data-stu-id="95b05-216">This request is called a *preflight request*.</span></span> <span data-ttu-id="95b05-217">Prohlížeči můžete přeskočit předběžný požadavek, pokud jsou splněny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="95b05-217">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="95b05-218">Metoda žádosti je GET, HEAD nebo POST.</span><span class="sxs-lookup"><span data-stu-id="95b05-218">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="95b05-219">Aplikaci nelze nastavit hlavičku žádosti jiných než `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, nebo `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="95b05-219">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="95b05-220">`Content-Type` Záhlaví,-li nastavit, protože má jednu z jedné z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="95b05-220">The `Content-Type` header, if set, has one of the one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="95b05-221">Sada pravidel na hlavičky požadavku pro požadavek klienta se vztahuje na hlavičky, které aplikace nastaví voláním `setRequestHeader` na `XMLHttpRequest` objektu.</span><span class="sxs-lookup"><span data-stu-id="95b05-221">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="95b05-222">Specifikace CORS volá tyto hlavičky *vytvářet hlavičky požadavku*.</span><span class="sxs-lookup"><span data-stu-id="95b05-222">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="95b05-223">Toto pravidlo neplatí pro záhlaví můžete nastavit v prohlížeči, jako například `User-Agent`, `Host`, nebo `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="95b05-223">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="95b05-224">Následuje příklad předběžný požadavek:</span><span class="sxs-lookup"><span data-stu-id="95b05-224">The following is an example of a preflight request:</span></span>

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="95b05-225">Přípravné požadavek používá metodu HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="95b05-225">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="95b05-226">Obsahuje dva speciálními záhlavími:</span><span class="sxs-lookup"><span data-stu-id="95b05-226">It includes two special headers:</span></span>

* <span data-ttu-id="95b05-227">`Access-Control-Request-Method`: Metoda HTTP, který se použije pro aktuálního požadavku.</span><span class="sxs-lookup"><span data-stu-id="95b05-227">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="95b05-228">`Access-Control-Request-Headers`: Seznam hlaviček požadavků, které aplikace nastaví u aktuálního požadavku.</span><span class="sxs-lookup"><span data-stu-id="95b05-228">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="95b05-229">Jak bylo uvedeno dříve, to nezahrnuje hlavičky, které nastaví v prohlížeči, jako například `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="95b05-229">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="95b05-230">Předběžný požadavek CORS patří `Access-Control-Request-Headers` hlavičky, která označuje hlavičky, které jsou odeslány pomocí aktuálního požadavku na server.</span><span class="sxs-lookup"><span data-stu-id="95b05-230">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="95b05-231">Chcete-li povolit konkrétní hlavičky, zavolejte <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="95b05-231">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="95b05-232">Chcete-li povolit všechny hlavičky žádosti, vytvářet volání <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="95b05-232">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="95b05-233">Nejsou zcela konzistentní v tom, jak je nastavit prohlížeče `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="95b05-233">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="95b05-234">Pokud nastavíte záhlaví na něco jiného než `"*"` (nebo použijte <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), by měl obsahovat alespoň `Accept`, `Content-Type`, a `Origin`, a navíc jakékoli vlastní hlavičky, které chcete podporovat.</span><span class="sxs-lookup"><span data-stu-id="95b05-234">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="95b05-235">Následuje příklad odpovědi pro předběžný požadavek (za předpokladu, že server umožňuje žádosti):</span><span class="sxs-lookup"><span data-stu-id="95b05-235">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="95b05-236">Odpověď obsahuje `Access-Control-Allow-Methods` hlavičku, která uvádí povolené metody a volitelně `Access-Control-Allow-Headers` hlavičky, která uvádí povolené hlavičky.</span><span class="sxs-lookup"><span data-stu-id="95b05-236">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="95b05-237">Pokud je předběžný požadavek úspěšné, prohlížeč odesílá aktuálního požadavku.</span><span class="sxs-lookup"><span data-stu-id="95b05-237">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="95b05-238">Pokud je předběžný požadavek, vrátí aplikaci *200 OK* odpovědi ale nebude odesílat hlavičky CORS zpět.</span><span class="sxs-lookup"><span data-stu-id="95b05-238">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="95b05-239">Prohlížeč proto nebude se pokoušet žádosti nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="95b05-239">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="95b05-240">Nastavit čas vypršení platnosti předběžné</span><span class="sxs-lookup"><span data-stu-id="95b05-240">Set the preflight expiration time</span></span>

<span data-ttu-id="95b05-241">`Access-Control-Max-Age` Záhlaví Určuje, jak dlouho může do mezipaměti odpovědi pro předběžný požadavek.</span><span class="sxs-lookup"><span data-stu-id="95b05-241">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="95b05-242">Chcete-li nastavit tuto hlavičku, zavolejte <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="95b05-242">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a><span data-ttu-id="95b05-243">Jak funguje CORS</span><span class="sxs-lookup"><span data-stu-id="95b05-243">How CORS works</span></span>

<span data-ttu-id="95b05-244">Tato část popisuje, co se stane, že v požadavku CORS na úrovni zprávy HTTP.</span><span class="sxs-lookup"><span data-stu-id="95b05-244">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="95b05-245">Je důležité pochopit, jak CORS funguje tak, aby zásady CORS můžete nakonfigurovat správně a ladit, když dojde k neočekávané chování.</span><span class="sxs-lookup"><span data-stu-id="95b05-245">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="95b05-246">Specifikace CORS zavádí několik nové hlavičky protokolu HTTP, které umožňují požadavky cross-origin.</span><span class="sxs-lookup"><span data-stu-id="95b05-246">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="95b05-247">Pokud je prohlížeč podporuje CORS, nastaví tyto hlavičky automaticky pro požadavky cross-origin.</span><span class="sxs-lookup"><span data-stu-id="95b05-247">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="95b05-248">Vlastní kód jazyka JavaScript není vyžadován k povolení sdílení CORS.</span><span class="sxs-lookup"><span data-stu-id="95b05-248">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="95b05-249">Následuje příklad žádosti nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="95b05-249">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="95b05-250">`Origin` Záhlaví obsahuje domény, lokality, která odeslala žádost:</span><span class="sxs-lookup"><span data-stu-id="95b05-250">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="95b05-251">Pokud server umožňuje, aby žádosti, nastaví `Access-Control-Allow-Origin` hlaviček v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="95b05-251">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="95b05-252">Hodnotu této hlavičky odpovídá buď `Origin` hlavička ze žádosti nebo je hodnota zástupného znaku `"*"`, což znamená, že jakýkoli původ je povoleno:</span><span class="sxs-lookup"><span data-stu-id="95b05-252">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="95b05-253">Pokud odpověď neobsahuje `Access-Control-Allow-Origin` hlavičky žádosti nepůvodního selže.</span><span class="sxs-lookup"><span data-stu-id="95b05-253">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="95b05-254">Konkrétně v prohlížeči zakazuje požadavku.</span><span class="sxs-lookup"><span data-stu-id="95b05-254">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="95b05-255">I v případě, že server vrátí úspěšné odpovědi, prohlížeč nevyužívá odpovědi k dispozici pro klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="95b05-255">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="95b05-256">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="95b05-256">Additional resources</span></span>

* [<span data-ttu-id="95b05-257">Prostředků mezi zdroji (CORS) pro sdílení obsahu</span><span class="sxs-lookup"><span data-stu-id="95b05-257">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
