---
title: Povolení žádostí napříč zdroji (CORS) v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak CORS jako standard pro povolení nebo odmítnutí žádostí nepůvodního v aplikaci ASP.NET Core.
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2018
ms.locfileid: "41756746"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="41694-103">Povolení žádostí napříč zdroji (CORS) v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41694-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="41694-104">Podle [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), a [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="41694-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="41694-105">Zabezpečení prohlížečů brání zasílání požadavků AJAX na jinou doménu na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="41694-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="41694-106">Toto omezení je volána *zásada stejného zdroje*a brání škodlivým webům ve čtení citlivých dat z jiné lokality.</span><span class="sxs-lookup"><span data-stu-id="41694-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="41694-107">Ale v některých případech můžete chtít nechat jiných lokalit, zkontrolujte požadavky cross-origin do webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="41694-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="41694-108">[Mezi sdílení zdrojů původu](http://www.w3.org/TR/cors/) (CORS) je standard W3C, která umožňuje server zmírnit zásadu stejného zdroje.</span><span class="sxs-lookup"><span data-stu-id="41694-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="41694-109">Pomocí CORS, server explicitně můžou některé požadavky cross-origin zatímco jiné odmítnout.</span><span class="sxs-lookup"><span data-stu-id="41694-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="41694-110">CORS je bezpečnější a flexibilnější, než starší techniky, jako [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="41694-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="41694-111">Toto téma ukazuje, jak povolit CORS v aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="41694-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="41694-112">Co je "stejné zdroj"?</span><span class="sxs-lookup"><span data-stu-id="41694-112">What is "same origin"?</span></span>

<span data-ttu-id="41694-113">Dvě adresy URL mají stejný původ, pokud mají stejné schémata, hostitele a porty.</span><span class="sxs-lookup"><span data-stu-id="41694-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="41694-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="41694-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="41694-115">Tyto dvě adresy URL mají stejného původu:</span><span class="sxs-lookup"><span data-stu-id="41694-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="41694-116">Tyto adresy URL mají různé zdroje než ta předchozí dvě:</span><span class="sxs-lookup"><span data-stu-id="41694-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="41694-117">`http://example.net` -Jinou doménu</span><span class="sxs-lookup"><span data-stu-id="41694-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="41694-118">`http://www.example.com/foo.html` – Různé subdomény</span><span class="sxs-lookup"><span data-stu-id="41694-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="41694-119">`https://example.com/foo.html` -Jiné schéma</span><span class="sxs-lookup"><span data-stu-id="41694-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="41694-120">`http://example.com:9000/foo.html` -Jiný port</span><span class="sxs-lookup"><span data-stu-id="41694-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="41694-121">Aplikace Internet Explorer nezahrne port při porovnání zdrojů.</span><span class="sxs-lookup"><span data-stu-id="41694-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="enable-cors"></a><span data-ttu-id="41694-122">Povolení CORS</span><span class="sxs-lookup"><span data-stu-id="41694-122">Enable CORS</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="41694-123">Chcete-li nastavení CORS pro vaši aplikaci přidejte `Microsoft.AspNetCore.Cors` do svého projektu balíček.</span><span class="sxs-lookup"><span data-stu-id="41694-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

::: moniker-end

<span data-ttu-id="41694-124">Volání [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) v `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="41694-124">Call [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="41694-125">Povolení CORS s middlewaru</span><span class="sxs-lookup"><span data-stu-id="41694-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="41694-126">Postup pro povolení CORS, přidat CORS middleware do kanálu pomocí požadavku `UseCors` – metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="41694-126">To enable CORS, add the CORS middleware to the request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="41694-127">CORS middleware musí předcházet všechny koncové body definované ve vaší aplikaci ve které chcete podporovat požadavky cross-origin (například před voláním `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="41694-127">The CORS middleware must precede any defined endpoints in your app where you want to support cross-origin requests (For example, before any call to `UseMvc`).</span></span>

<span data-ttu-id="41694-128">Zásady nepůvodního lze zadat při přidání pomocí middleware CORS [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) třídy.</span><span class="sxs-lookup"><span data-stu-id="41694-128">A cross-origin policy can be specified when adding the CORS middleware using the [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) class.</span></span> <span data-ttu-id="41694-129">Existují dva způsoby, jak to provést.</span><span class="sxs-lookup"><span data-stu-id="41694-129">There are two ways to do this.</span></span> <span data-ttu-id="41694-130">První je volání `UseCors` pomocí výrazu lambda:</span><span class="sxs-lookup"><span data-stu-id="41694-130">The first is to call `UseCors` with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="41694-131">**Poznámka:** adresa URL musí být zadán bez koncové lomítko (`/`).</span><span class="sxs-lookup"><span data-stu-id="41694-131">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="41694-132">Pokud adresa URL se ukončí s `/`, vrátí porovnání `false` a nevrátí se žádné záhlaví.</span><span class="sxs-lookup"><span data-stu-id="41694-132">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="41694-133">Používá výraz lambda `CorsPolicyBuilder` objektu.</span><span class="sxs-lookup"><span data-stu-id="41694-133">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="41694-134">Seznam najdete [možnosti konfigurace](#cors-policy-options) dále v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="41694-134">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="41694-135">V tomto příkladu, zásada umožňuje požadavky cross-origin z `http://example.com` a žádné jiné zdroje.</span><span class="sxs-lookup"><span data-stu-id="41694-135">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="41694-136">CorsPolicyBuilder má rozhraní API fluent, takže můžete vytvořit řetězové volání metody:</span><span class="sxs-lookup"><span data-stu-id="41694-136">CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="41694-137">Druhý postup je definovat jeden nebo více s názvem zásady CORS, a potom vyberte zásady podle názvu v době běhu.</span><span class="sxs-lookup"><span data-stu-id="41694-137">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="41694-138">V tomto příkladu přidá zásada CORS, s názvem "AllowSpecificOrigin".</span><span class="sxs-lookup"><span data-stu-id="41694-138">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="41694-139">Vyberte zásadu, předejte název, který má `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="41694-139">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="41694-140">Povolení CORS v aplikaci MVC</span><span class="sxs-lookup"><span data-stu-id="41694-140">Enabling CORS in MVC</span></span>

<span data-ttu-id="41694-141">Chcete-li použít konkrétní CORS na akce na kontroler nebo globálně pro všechny řadiče můžete také použít MVC.</span><span class="sxs-lookup"><span data-stu-id="41694-141">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="41694-142">Při použití MVC k povolení CORS se používají stejné CORS služby, ale není middlewarem CORS.</span><span class="sxs-lookup"><span data-stu-id="41694-142">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="41694-143">Za akci</span><span class="sxs-lookup"><span data-stu-id="41694-143">Per action</span></span>

<span data-ttu-id="41694-144">K určení přidat zásadu CORS pro určité akce `[EnableCors]` atribut na akci.</span><span class="sxs-lookup"><span data-stu-id="41694-144">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="41694-145">Zadejte název zásady.</span><span class="sxs-lookup"><span data-stu-id="41694-145">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="41694-146">Na kontroler</span><span class="sxs-lookup"><span data-stu-id="41694-146">Per controller</span></span>

<span data-ttu-id="41694-147">Chcete-li určit zásady CORS pro konkrétní ovladač přidat `[EnableCors]` atribut třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="41694-147">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="41694-148">Zadejte název zásady.</span><span class="sxs-lookup"><span data-stu-id="41694-148">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="41694-149">Globálně</span><span class="sxs-lookup"><span data-stu-id="41694-149">Globally</span></span>

<span data-ttu-id="41694-150">Můžete povolit CORS globálně pro všechny řadiče tak, že přidáte `CorsAuthorizationFilterFactory` filtr do kolekce globálních filtrů:</span><span class="sxs-lookup"><span data-stu-id="41694-150">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="41694-151">Pořadí priority je: akci, kontroler, globální.</span><span class="sxs-lookup"><span data-stu-id="41694-151">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="41694-152">Zásady na úrovni akce mají přednost zásady na úrovni kontroleru a zásad na úrovni kontroleru přednost před globální zásady.</span><span class="sxs-lookup"><span data-stu-id="41694-152">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="41694-153">Zákazu sdílení CORS</span><span class="sxs-lookup"><span data-stu-id="41694-153">Disable CORS</span></span>

<span data-ttu-id="41694-154">Chcete-li zakázat CORS pro kontroler nebo akce, použijte `[DisableCors]` atribut.</span><span class="sxs-lookup"><span data-stu-id="41694-154">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="41694-155">Možnosti zásad CORS</span><span class="sxs-lookup"><span data-stu-id="41694-155">CORS policy options</span></span>

<span data-ttu-id="41694-156">Tato část popisuje různé možnosti, které můžete nastavit zásadu CORS.</span><span class="sxs-lookup"><span data-stu-id="41694-156">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="41694-157">Nastavte povolené zdroje</span><span class="sxs-lookup"><span data-stu-id="41694-157">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="41694-158">Nastavte povolené metody HTTP</span><span class="sxs-lookup"><span data-stu-id="41694-158">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="41694-159">Nastavit hlavičku povolené žádosti</span><span class="sxs-lookup"><span data-stu-id="41694-159">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="41694-160">Nastavit hlavičky vystavené odpovědi</span><span class="sxs-lookup"><span data-stu-id="41694-160">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="41694-161">Přihlašovací údaje v požadavky cross-origin</span><span class="sxs-lookup"><span data-stu-id="41694-161">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="41694-162">Nastavit čas vypršení platnosti předběžné</span><span class="sxs-lookup"><span data-stu-id="41694-162">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="41694-163">Pro některé možnosti, může být užitečné přečíst [funguje jak CORS](#how-cors-works) první.</span><span class="sxs-lookup"><span data-stu-id="41694-163">For some options, it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="41694-164">Nastavte povolené zdroje</span><span class="sxs-lookup"><span data-stu-id="41694-164">Set the allowed origins</span></span>

<span data-ttu-id="41694-165">Chcete-li povolit jeden nebo více konkrétních zdrojů:</span><span class="sxs-lookup"><span data-stu-id="41694-165">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="41694-166">Chcete-li povolit všechny původy:</span><span class="sxs-lookup"><span data-stu-id="41694-166">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="41694-167">Pečlivě zvažte předtím, než žádosti z původu.</span><span class="sxs-lookup"><span data-stu-id="41694-167">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="41694-168">To znamená, že doslova všechny weby mohl provádět volání AJAX do vašeho rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="41694-168">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="41694-169">Nastavte povolené metody HTTP</span><span class="sxs-lookup"><span data-stu-id="41694-169">Set the allowed HTTP methods</span></span>

<span data-ttu-id="41694-170">Chcete-li povolit všechny metody HTTP:</span><span class="sxs-lookup"><span data-stu-id="41694-170">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="41694-171">Tímto je ovlivněn přípravné požadavků a hlavičky Access-ovládacího prvku-Allow-Methods.</span><span class="sxs-lookup"><span data-stu-id="41694-171">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="41694-172">Nastavit hlavičku povolené žádosti</span><span class="sxs-lookup"><span data-stu-id="41694-172">Set the allowed request headers</span></span>

<span data-ttu-id="41694-173">Předběžný požadavek CORS může obsahovat hlavičku Access-Control-Request-Headers, výpis hlavičky protokolu HTTP, nastavte aplikací (takzvaný ", vytvářet hlavičky žádosti").</span><span class="sxs-lookup"><span data-stu-id="41694-173">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="41694-174">Na seznamu povolených IP adres konkrétní záhlaví:</span><span class="sxs-lookup"><span data-stu-id="41694-174">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="41694-175">Chcete-li povolit všechny vytvářet hlavičky žádosti:</span><span class="sxs-lookup"><span data-stu-id="41694-175">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="41694-176">Prohlížeče nejsou zcela konzistentní v tom, jak nastavují Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="41694-176">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="41694-177">Pokud nastavíte záhlaví na něco jiného než "\*", měli byste zahrnout alespoň "přijímat", "content-type" a "zdroj" a navíc jakékoli vlastní hlavičky, které chcete podporovat.</span><span class="sxs-lookup"><span data-stu-id="41694-177">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="41694-178">Nastavit hlavičky vystavené odpovědi</span><span class="sxs-lookup"><span data-stu-id="41694-178">Set the exposed response headers</span></span>

<span data-ttu-id="41694-179">Ve výchozím prohlížeči nezveřejňuje všechny hlavičky odpovědí do aplikace.</span><span class="sxs-lookup"><span data-stu-id="41694-179">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="41694-180">(Viz [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Hlavičky odpovědi, které jsou k dispozici ve výchozím nastavení jsou:</span><span class="sxs-lookup"><span data-stu-id="41694-180">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="41694-181">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="41694-181">Cache-Control</span></span>

* <span data-ttu-id="41694-182">Jazyk obsahu</span><span class="sxs-lookup"><span data-stu-id="41694-182">Content-Language</span></span>

* <span data-ttu-id="41694-183">Typ obsahu</span><span class="sxs-lookup"><span data-stu-id="41694-183">Content-Type</span></span>

* <span data-ttu-id="41694-184">Vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="41694-184">Expires</span></span>

* <span data-ttu-id="41694-185">Datum poslední změny</span><span class="sxs-lookup"><span data-stu-id="41694-185">Last-Modified</span></span>

* <span data-ttu-id="41694-186">Direktiva pragma</span><span class="sxs-lookup"><span data-stu-id="41694-186">Pragma</span></span>

<span data-ttu-id="41694-187">Specifikace CORS volá tyto *hlavičky odpovědi jednoduché*.</span><span class="sxs-lookup"><span data-stu-id="41694-187">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="41694-188">Pro zpřístupnění dalších hlaviček pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="41694-188">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="41694-189">Přihlašovací údaje v požadavky cross-origin</span><span class="sxs-lookup"><span data-stu-id="41694-189">Credentials in cross-origin requests</span></span>

<span data-ttu-id="41694-190">Přihlašovací údaje vyžadují speciální zacházení v požadavku CORS.</span><span class="sxs-lookup"><span data-stu-id="41694-190">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="41694-191">Ve výchozím nastavení nebude prohlížeč odeslat žádné přihlašovací údaje se žádostí nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="41694-191">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="41694-192">Přihlašovací údaje zahrnují soubory cookie, jakož i schémat ověřování protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="41694-192">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="41694-193">Odesílá pověření s žádostí nepůvodního zdroje, musíte nastavit klienta XMLHttpRequest.withCredentials na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="41694-193">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="41694-194">Přímo pomocí rozhraní XMLHttpRequest:</span><span class="sxs-lookup"><span data-stu-id="41694-194">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="41694-195">V jQuery:</span><span class="sxs-lookup"><span data-stu-id="41694-195">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="41694-196">Na serveru navíc musíte povolit přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="41694-196">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="41694-197">Povolit přihlašovací údaje nepůvodního zdroje:</span><span class="sxs-lookup"><span data-stu-id="41694-197">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="41694-198">Odpověď HTTP teď bude obsahovat hlavičku přístup – ovládací prvek-Allow-Credentials, která sděluje prohlížeči, že server umožňuje přihlašovací údaje pro žádosti nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="41694-198">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="41694-199">Pokud prohlížeč odesílá pověření, ale odpověď neobsahuje platnou hlavičku přístup – ovládací prvek-Allow-Credentials, prohlížeči nebude vystavení odpovědi do aplikace a požadavek AJAX selže.</span><span class="sxs-lookup"><span data-stu-id="41694-199">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="41694-200">Buďte opatrní při povolování pověření nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="41694-200">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="41694-201">Web v jiné doméně odeslat do aplikace jménem uživatele bez vědomí uživatele pověření přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="41694-201">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="41694-202">Specifikace CORS také uvádí nastavení počátky k `"*"` (všechny původy) je neplatná-li `Access-Control-Allow-Credentials` záhlaví je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="41694-202">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="41694-203">Nastavit čas vypršení platnosti předběžné</span><span class="sxs-lookup"><span data-stu-id="41694-203">Set the preflight expiration time</span></span>

<span data-ttu-id="41694-204">Hlavička Access – ovládací prvek-Max-Age Určuje, jak dlouho může do mezipaměti odpovědi pro předběžný požadavek.</span><span class="sxs-lookup"><span data-stu-id="41694-204">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="41694-205">Nastavení této hlavičky:</span><span class="sxs-lookup"><span data-stu-id="41694-205">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="41694-206">Jak funguje CORS</span><span class="sxs-lookup"><span data-stu-id="41694-206">How CORS works</span></span>

<span data-ttu-id="41694-207">Tato část popisuje, co se stane, že v požadavku CORS na úrovni zprávy HTTP.</span><span class="sxs-lookup"><span data-stu-id="41694-207">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="41694-208">Je důležité pochopit, jak CORS funguje tak, aby zásady CORS můžete nakonfigurovat správně a ladit, když dojde k neočekávané chování.</span><span class="sxs-lookup"><span data-stu-id="41694-208">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="41694-209">Specifikace CORS zavádí několik nové hlavičky protokolu HTTP, které umožňují požadavky cross-origin.</span><span class="sxs-lookup"><span data-stu-id="41694-209">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="41694-210">Pokud je prohlížeč podporuje CORS, nastaví tyto hlavičky automaticky pro požadavky cross-origin.</span><span class="sxs-lookup"><span data-stu-id="41694-210">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="41694-211">Vlastní kód jazyka JavaScript není vyžadován k povolení sdílení CORS.</span><span class="sxs-lookup"><span data-stu-id="41694-211">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="41694-212">Tady je příklad žádosti nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="41694-212">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="41694-213">`Origin` Záhlaví obsahuje domény, lokality, která odeslala žádost:</span><span class="sxs-lookup"><span data-stu-id="41694-213">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="41694-214">Pokud server umožňuje, aby žádosti, nastaví hlavičky Access-Control-Allow-Origin v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="41694-214">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="41694-215">Hodnotu této hlavičky odpovídá hlavička počátečního z požadavku nebo hodnota zástupný znak "\*", což znamená, že jakýkoli původ je povoleno:</span><span class="sxs-lookup"><span data-stu-id="41694-215">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="41694-216">Pokud odpověď nebude obsahovat hlavičky Access-Control-Allow-Origin, požadavek AJAX selže.</span><span class="sxs-lookup"><span data-stu-id="41694-216">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="41694-217">Konkrétně v prohlížeči zakazuje požadavku.</span><span class="sxs-lookup"><span data-stu-id="41694-217">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="41694-218">I v případě, že server vrátí úspěšné odpovědi, prohlížeč nevyužívá odpovědi k dispozici pro klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="41694-218">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="41694-219">Předběžných požadavků</span><span class="sxs-lookup"><span data-stu-id="41694-219">Preflight Requests</span></span>

<span data-ttu-id="41694-220">U některých požadavků CORS prohlížeč odešle požadavek další, nazývá "předběžný požadavek", předtím, než odešle skutečnou žádost pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="41694-220">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="41694-221">Prohlížeči můžete přeskočit předběžný požadavek, pokud jsou splněny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="41694-221">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="41694-222">Metoda žádosti je GET, HEAD nebo POST, a</span><span class="sxs-lookup"><span data-stu-id="41694-222">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="41694-223">Aplikace není nastavena záhlaví požadavku než přijmout, Accept-Language, jazyka obsahu Content-Type nebo poslední-Event-ID, a</span><span class="sxs-lookup"><span data-stu-id="41694-223">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="41694-224">Hlavička Content-Type (Pokud nastavit) je jedním z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="41694-224">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="41694-225">Application/x--www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="41694-225">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="41694-226">multipart/formuláře data</span><span class="sxs-lookup"><span data-stu-id="41694-226">multipart/form-data</span></span>

  * <span data-ttu-id="41694-227">text/plain</span><span class="sxs-lookup"><span data-stu-id="41694-227">text/plain</span></span>

<span data-ttu-id="41694-228">Pravidlo o hlavičky požadavku se vztahuje na hlavičky, které aplikace nastaví voláním setRequestHeader na objekt XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="41694-228">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="41694-229">(Specifikaci CORS volá tyto "Autor hlavičky žádosti".) Toto pravidlo neplatí pro hlavičky, které můžete nastavit v prohlížeči, jako je například uživatelského agenta, hostitele nebo Content-Length.</span><span class="sxs-lookup"><span data-stu-id="41694-229">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="41694-230">Tady je příklad předběžný požadavek:</span><span class="sxs-lookup"><span data-stu-id="41694-230">Here is an example of a preflight request:</span></span>

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="41694-231">Přípravné požadavek používá metodu HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="41694-231">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="41694-232">Obsahuje dva speciálními záhlavími:</span><span class="sxs-lookup"><span data-stu-id="41694-232">It includes two special headers:</span></span>

* <span data-ttu-id="41694-233">Access-Control-Request-Method: Metoda HTTP, který se použije pro aktuálního požadavku.</span><span class="sxs-lookup"><span data-stu-id="41694-233">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="41694-234">Access-Control-Request-Headers: Seznam hlaviček požadavků, které aplikace nastavena na skutečnou žádost.</span><span class="sxs-lookup"><span data-stu-id="41694-234">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="41694-235">(Znovu to nezahrnuje hlavičky, které nastaví prohlížeče.)</span><span class="sxs-lookup"><span data-stu-id="41694-235">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="41694-236">Tady je příklad odpovědi, za předpokladu, že server umožňuje žádosti:</span><span class="sxs-lookup"><span data-stu-id="41694-236">Here is an example response, assuming that the server allows the request:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="41694-237">Odpověď obsahuje hlavičku přístup – ovládací prvek-Allow-Methods, který obsahuje seznam povolených metod a volitelně hlavičky Access-Control-povolit-Headers, která zobrazuje povolené hlavičky.</span><span class="sxs-lookup"><span data-stu-id="41694-237">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="41694-238">Pokud je předběžný požadavek úspěšné, prohlížeč odesílá skutečnou žádost, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="41694-238">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
