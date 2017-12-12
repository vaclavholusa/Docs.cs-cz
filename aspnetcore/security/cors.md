---
title: "Povolení žádostí napříč zdroji (CORS)"
author: rick-anderson
description: "Toto téma představuje CORS jako standard pro povolení nebo odmítnutí žádostí napříč zdroji v aplikaci ASP.NET Core."
keywords: "ASP.NET Core, CORS, křížové počátek"
ms.author: riande
manager: wpickett
ms.date: 05/17/2017
ms.topic: article
ms.assetid: f9d95e88-4d7e-4d0c-a8e1-47de1128d505
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cors
ms.openlocfilehash: 5398b6ad6531710de2b8000cb368e5fa607ae7ff
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="enabling-cross-origin-requests-cors"></a><span data-ttu-id="020ae-104">Povolení žádostí napříč zdroji (CORS)</span><span class="sxs-lookup"><span data-stu-id="020ae-104">Enabling Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="020ae-105">Podle [Karel Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), a [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="020ae-105">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="020ae-106">Zabezpečení prohlížeče brání provedení požadavky AJAX do jiné domény na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="020ae-106">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="020ae-107">Toto omezení se nazývá *zásady stejného původu*a zabrání škodlivé weby čtení citlivá data z jiné lokality.</span><span class="sxs-lookup"><span data-stu-id="020ae-107">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="020ae-108">Ale v některých případech můžete chtít nechat ostatních lokalit, zkontrolujte žádostí napříč zdroji do webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="020ae-108">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="020ae-109">[Mezi sdílení prostředků původu](http://www.w3.org/TR/cors/) (CORS) je standard W3C, který umožňuje serveru zmírnit zásady stejného původu.</span><span class="sxs-lookup"><span data-stu-id="020ae-109">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="020ae-110">Pomocí CORS, server explicitně povolit některé žádostí napříč zdroji při odmítnutí ostatní.</span><span class="sxs-lookup"><span data-stu-id="020ae-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="020ae-111">CORS, jako je bezpečnější a flexibilnější než dřívější techniky [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="020ae-111">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="020ae-112">Toto téma ukazuje postup povolení CORS v aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="020ae-112">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="020ae-113">Co je "stejné původ"?</span><span class="sxs-lookup"><span data-stu-id="020ae-113">What is "same origin"?</span></span>

<span data-ttu-id="020ae-114">Dvou adres URL mají stejný původ v případě, že mají stejné schémata, hostitele a porty.</span><span class="sxs-lookup"><span data-stu-id="020ae-114">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="020ae-115">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="020ae-115">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="020ae-116">Tyto dvě adresy URL mají stejný původ:</span><span class="sxs-lookup"><span data-stu-id="020ae-116">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="020ae-117">Tyto adresy URL mít různého původu než předchozí dva:</span><span class="sxs-lookup"><span data-stu-id="020ae-117">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="020ae-118">`http://example.net`-Jiné domény</span><span class="sxs-lookup"><span data-stu-id="020ae-118">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="020ae-119">`http://www.example.com/foo.html`-Různých subdomény</span><span class="sxs-lookup"><span data-stu-id="020ae-119">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="020ae-120">`https://example.com/foo.html`-Jiné schéma</span><span class="sxs-lookup"><span data-stu-id="020ae-120">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="020ae-121">`http://example.com:9000/foo.html`-Jiný port</span><span class="sxs-lookup"><span data-stu-id="020ae-121">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="020ae-122">Internet Explorer nebere v úvahu port při porovnávání zdroje.</span><span class="sxs-lookup"><span data-stu-id="020ae-122">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="setting-up-cors"></a><span data-ttu-id="020ae-123">Nastavení CORS</span><span class="sxs-lookup"><span data-stu-id="020ae-123">Setting up CORS</span></span>

<span data-ttu-id="020ae-124">K nastavení CORS pro vaši aplikaci přidat `Microsoft.AspNetCore.Cors` balíčku do projektu.</span><span class="sxs-lookup"><span data-stu-id="020ae-124">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

<span data-ttu-id="020ae-125">Přidání služeb CORS v souboru Startup.cs:</span><span class="sxs-lookup"><span data-stu-id="020ae-125">Add the CORS services in Startup.cs:</span></span>

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="020ae-126">Povolení CORS s middlewaru.</span><span class="sxs-lookup"><span data-stu-id="020ae-126">Enabling CORS with middleware</span></span>

<span data-ttu-id="020ae-127">Chcete-li přidat CORS pro celou aplikaci CORS middleware do kanálu vaší žádosti o pomocí `UseCors` metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="020ae-127">To enable CORS for your entire application add the CORS middleware to your request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="020ae-128">Všimněte si, že CORS middleware musí předcházet žádné koncové body definované ve vaší aplikaci, která chcete podporovat žádostí napříč zdroji (např.)</span><span class="sxs-lookup"><span data-stu-id="020ae-128">Note that the CORS middleware must precede any defined endpoints in your app that you want to support cross-origin requests (ex.</span></span> <span data-ttu-id="020ae-129">Před voláním jakékoli `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="020ae-129">before any call to `UseMvc`).</span></span>

<span data-ttu-id="020ae-130">Při přidávání pomocí middleware CORS můžete zadat zásady cross-origin `CorsPolicyBuilder` třídy.</span><span class="sxs-lookup"><span data-stu-id="020ae-130">You can specify a cross-origin policy when adding the CORS middleware using the `CorsPolicyBuilder` class.</span></span> <span data-ttu-id="020ae-131">Chcete-li to provést dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="020ae-131">There are two ways to do this.</span></span> <span data-ttu-id="020ae-132">První je volání UseCors s lambda:</span><span class="sxs-lookup"><span data-stu-id="020ae-132">The first is to call UseCors with a lambda:</span></span>

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="020ae-133">**Poznámka:** bez koncové lomítko musíte zadat adresu URL (`/`).</span><span class="sxs-lookup"><span data-stu-id="020ae-133">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="020ae-134">Pokud adresu URL ukončí s `/`, vrátí porovnání `false` , které budou vráceny žádné záhlaví.</span><span class="sxs-lookup"><span data-stu-id="020ae-134">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="020ae-135">Argument lambda trvá `CorsPolicyBuilder` objektu.</span><span class="sxs-lookup"><span data-stu-id="020ae-135">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="020ae-136">Seznam najdete [možnosti konfigurace](#cors-policy-options) dál v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="020ae-136">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="020ae-137">V tomto příkladu zásada umožňuje žádostí napříč zdroji z `http://example.com` a žádné jiné zdroje.</span><span class="sxs-lookup"><span data-stu-id="020ae-137">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="020ae-138">Všimněte si, že CorsPolicyBuilder obsahuje rozhraní fluent API, takže můžete řetězu volání metod:</span><span class="sxs-lookup"><span data-stu-id="020ae-138">Note that CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[Main](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="020ae-139">Druhý postup je definovat jeden nebo více s názvem zásady CORS, a potom vyberte zásady podle názvu v době běhu.</span><span class="sxs-lookup"><span data-stu-id="020ae-139">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[Main](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="020ae-140">Tento příklad přidá zásadu CORS s názvem "AllowSpecificOrigin".</span><span class="sxs-lookup"><span data-stu-id="020ae-140">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="020ae-141">Vyberte zásadu, předat název, který má `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="020ae-141">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="020ae-142">Povolení CORS v MVC</span><span class="sxs-lookup"><span data-stu-id="020ae-142">Enabling CORS in MVC</span></span>

<span data-ttu-id="020ae-143">Případně můžete MVC použít konkrétní CORS na každou akci, na jeden kontroler nebo globálně pro všechny řadiče.</span><span class="sxs-lookup"><span data-stu-id="020ae-143">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="020ae-144">Při použití MVC k povolení sdílení CORS se používají stejné CORS služby, ale není CORS middleware.</span><span class="sxs-lookup"><span data-stu-id="020ae-144">When using MVC to enable CORS the same CORS services are used, but the CORS middleware is not.</span></span>

### <a name="per-action"></a><span data-ttu-id="020ae-145">Na každou akci</span><span class="sxs-lookup"><span data-stu-id="020ae-145">Per action</span></span>

<span data-ttu-id="020ae-146">Zadejte přidat zásadu CORS pro určité akci `[EnableCors]` atribut akce.</span><span class="sxs-lookup"><span data-stu-id="020ae-146">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="020ae-147">Zadejte název zásady.</span><span class="sxs-lookup"><span data-stu-id="020ae-147">Specify the policy name.</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="020ae-148">Na jeden kontroler</span><span class="sxs-lookup"><span data-stu-id="020ae-148">Per controller</span></span>

<span data-ttu-id="020ae-149">Zadejte zásady CORS pro konkrétní řadič přidat `[EnableCors]` atributu do třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="020ae-149">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="020ae-150">Zadejte název zásady.</span><span class="sxs-lookup"><span data-stu-id="020ae-150">Specify the policy name.</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="020ae-151">Globálně</span><span class="sxs-lookup"><span data-stu-id="020ae-151">Globally</span></span>

<span data-ttu-id="020ae-152">Můžete povolit CORS globálně pro všechny řadiče přidáním `CorsAuthorizationFilterFactory` filtr do kolekce globálních filtrů:</span><span class="sxs-lookup"><span data-stu-id="020ae-152">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="020ae-153">Pořadí priorit je: akci, kontroler, globální.</span><span class="sxs-lookup"><span data-stu-id="020ae-153">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="020ae-154">Zásady na úrovni akce mají přednost před zásady na úrovni kontroleru a zásady na úrovni kontroleru mají přednost před globální zásady.</span><span class="sxs-lookup"><span data-stu-id="020ae-154">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="020ae-155">Zakázat CORS</span><span class="sxs-lookup"><span data-stu-id="020ae-155">Disable CORS</span></span>

<span data-ttu-id="020ae-156">Chcete-li zakázat CORS pro kontroler nebo akce, použijte `[DisableCors]` atribut.</span><span class="sxs-lookup"><span data-stu-id="020ae-156">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="020ae-157">Možnosti zásad CORS</span><span class="sxs-lookup"><span data-stu-id="020ae-157">CORS policy options</span></span>

<span data-ttu-id="020ae-158">Tato část popisuje různé možnosti, které můžete nastavit v zásadu CORS.</span><span class="sxs-lookup"><span data-stu-id="020ae-158">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="020ae-159">Nastavit povolené zdroje</span><span class="sxs-lookup"><span data-stu-id="020ae-159">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="020ae-160">Nastavení povolených metod HTTP</span><span class="sxs-lookup"><span data-stu-id="020ae-160">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="020ae-161">Nastavit hlavičky povolených požadavků</span><span class="sxs-lookup"><span data-stu-id="020ae-161">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="020ae-162">Nastavit hlavičky zveřejněné odpovědi</span><span class="sxs-lookup"><span data-stu-id="020ae-162">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="020ae-163">Přihlašovací údaje v žádostí napříč zdroji</span><span class="sxs-lookup"><span data-stu-id="020ae-163">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="020ae-164">Nastavte hodnotu doby předběžných vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="020ae-164">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="020ae-165">Pro některé možnosti může být užitečné číst [funguje jak CORS](#how-cors-works) první.</span><span class="sxs-lookup"><span data-stu-id="020ae-165">For some options it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="020ae-166">Nastavit povolené zdroje</span><span class="sxs-lookup"><span data-stu-id="020ae-166">Set the allowed origins</span></span>

<span data-ttu-id="020ae-167">Chcete-li povolit jeden nebo více konkrétních zdroje:</span><span class="sxs-lookup"><span data-stu-id="020ae-167">To allow one or more specific origins:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="020ae-168">Chcete-li povolit všechny původy:</span><span class="sxs-lookup"><span data-stu-id="020ae-168">To allow all origins:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="020ae-169">Pečlivě zvažte, před povolením požadavky od jakýkoli původ.</span><span class="sxs-lookup"><span data-stu-id="020ae-169">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="020ae-170">Znamená to, že názvy všech webu provádět volání AJAX k vašemu rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="020ae-170">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="020ae-171">Nastavení povolených metod HTTP</span><span class="sxs-lookup"><span data-stu-id="020ae-171">Set the allowed HTTP methods</span></span>

<span data-ttu-id="020ae-172">Chcete-li povolit všechny metody HTTP:</span><span class="sxs-lookup"><span data-stu-id="020ae-172">To allow all HTTP methods:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="020ae-173">To ovlivní letu předběžné požadavky a záhlaví přístupu – ovládací prvek-Allow-Methods.</span><span class="sxs-lookup"><span data-stu-id="020ae-173">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="020ae-174">Nastavit hlavičky povolených požadavků</span><span class="sxs-lookup"><span data-stu-id="020ae-174">Set the allowed request headers</span></span>

<span data-ttu-id="020ae-175">Předběžný požadavek CORS může obsahovat hlavičku Access-Control-Request-Headers výpis hlavičky protokolu HTTP, které jsou nastaveny aplikací (takzvaný "vytvářet hlavičky žádosti").</span><span class="sxs-lookup"><span data-stu-id="020ae-175">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="020ae-176">Pro konkrétní hlavičky seznamu povolených IP adres:</span><span class="sxs-lookup"><span data-stu-id="020ae-176">To whitelist specific headers:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="020ae-177">Umožňuje vytvářet všechny hlavičky žádosti:</span><span class="sxs-lookup"><span data-stu-id="020ae-177">To allow all author request headers:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="020ae-178">Nejsou prohlížeče zcela v souladu v tom, jak nastavují Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="020ae-178">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="020ae-179">Pokud nastavíte hlavičky k ničemu jiné než "*", by měla obsahovat alespoň "přijmout", "content-type" a "Původ" a jakékoli vlastní hlavičky, které chcete podporovat.</span><span class="sxs-lookup"><span data-stu-id="020ae-179">If you set headers to anything other than "*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="020ae-180">Nastavit hlavičky zveřejněné odpovědi</span><span class="sxs-lookup"><span data-stu-id="020ae-180">Set the exposed response headers</span></span>

<span data-ttu-id="020ae-181">Ve výchozím prohlížeči nevystavuje všechny hlavičky odpovědi do aplikace.</span><span class="sxs-lookup"><span data-stu-id="020ae-181">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="020ae-182">(Viz [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) Hlavičky odpovědi, které jsou k dispozici ve výchozím nastavení jsou:</span><span class="sxs-lookup"><span data-stu-id="020ae-182">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="020ae-183">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="020ae-183">Cache-Control</span></span>

* <span data-ttu-id="020ae-184">Obsah – jazyk</span><span class="sxs-lookup"><span data-stu-id="020ae-184">Content-Language</span></span>

* <span data-ttu-id="020ae-185">Typ obsahu</span><span class="sxs-lookup"><span data-stu-id="020ae-185">Content-Type</span></span>

* <span data-ttu-id="020ae-186">Vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="020ae-186">Expires</span></span>

* <span data-ttu-id="020ae-187">Poslední úpravy</span><span class="sxs-lookup"><span data-stu-id="020ae-187">Last-Modified</span></span>

* <span data-ttu-id="020ae-188">Direktiva pragma</span><span class="sxs-lookup"><span data-stu-id="020ae-188">Pragma</span></span>

<span data-ttu-id="020ae-189">Specifikace CORS volá tyto *hlavičky odpovědi jednoduché*.</span><span class="sxs-lookup"><span data-stu-id="020ae-189">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="020ae-190">Pro zpřístupnění jiných hlavičky pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="020ae-190">To make other headers available to the application:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="020ae-191">Přihlašovací údaje v žádostí napříč zdroji</span><span class="sxs-lookup"><span data-stu-id="020ae-191">Credentials in cross-origin requests</span></span>

<span data-ttu-id="020ae-192">Přihlašovací údaje vyžadují zvláštní zpracování v požadavek CORS.</span><span class="sxs-lookup"><span data-stu-id="020ae-192">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="020ae-193">Ve výchozím prohlížeči neodešle všechny přihlašovací údaje s žádostí napříč zdroji.</span><span class="sxs-lookup"><span data-stu-id="020ae-193">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="020ae-194">Přihlašovací údaje zahrnují soubory cookie, jakož i schémat ověřování protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="020ae-194">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="020ae-195">K odeslání pověření s žádostí napříč zdroji, musí klient nastavit XMLHttpRequest.withCredentials na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="020ae-195">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="020ae-196">Pomocí rozhraní XMLHttpRequest přímo:</span><span class="sxs-lookup"><span data-stu-id="020ae-196">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="020ae-197">V jQuery:</span><span class="sxs-lookup"><span data-stu-id="020ae-197">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="020ae-198">Kromě toho serveru musí umožňovat přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="020ae-198">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="020ae-199">Chcete-li povolit cross-origin přihlašovací údaje:</span><span class="sxs-lookup"><span data-stu-id="020ae-199">To allow cross-origin credentials:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="020ae-200">Odpověď HTTP bude teď obsahovat přístup – ovládací prvek-Allow-Credentials záhlaví, která sděluje prohlížeči, že server umožňuje přihlašovací údaje pro žádost o nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="020ae-200">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="020ae-201">Pokud prohlížeč odesílá přihlašovací údaje, ale odpověď neobsahuje platný přístup – ovládací prvek-Allow-Credentials záhlaví, prohlížeče nebude vystavit odpověď na žádost a vyvolá chybu požadavek AJAX.</span><span class="sxs-lookup"><span data-stu-id="020ae-201">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="020ae-202">Pečlivě velmi o povolení cross-origin přihlašovací údaje, protože znamená to, že web v jiné doméně může odesílat pověření přihlášeného uživatele do vaší aplikace jménem uživatele, aniž by uživatel znal.</span><span class="sxs-lookup"><span data-stu-id="020ae-202">Be very careful about allowing cross-origin credentials, because it means a website at another domain can send a logged-in user’s credentials to your app on the user’s behalf, without the user being aware.</span></span> <span data-ttu-id="020ae-203">Sdílení CORS spec také stavy tohoto nastavení počátky k "*" (všechny původy) je neplatný, pokud je k dispozici přístup – ovládací prvek-Allow-Credentials záhlaví.</span><span class="sxs-lookup"><span data-stu-id="020ae-203">The CORS spec also states that setting origins to "*" (all origins) is invalid if the Access-Control-Allow-Credentials header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="020ae-204">Nastavte hodnotu doby předběžných vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="020ae-204">Set the preflight expiration time</span></span>

<span data-ttu-id="020ae-205">Záhlaví přístupu – ovládací prvek-Max-Age Určuje, jak dlouho může do mezipaměti odpovědi na předběžný požadavek.</span><span class="sxs-lookup"><span data-stu-id="020ae-205">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="020ae-206">Chcete-li nastavit tuto hlavičku:</span><span class="sxs-lookup"><span data-stu-id="020ae-206">To set this header:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="020ae-207">Jak funguje CORS</span><span class="sxs-lookup"><span data-stu-id="020ae-207">How CORS works</span></span>

<span data-ttu-id="020ae-208">Tato část popisuje, co se stane, že v žádosti o CORS na úrovni zpráv protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="020ae-208">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="020ae-209">Je důležité pochopit, jak CORS funguje, takže můžete nakonfigurovat zásady CORS správně a vyřešit případné věcí nefungují podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="020ae-209">It’s important to understand how CORS works, so that you can configure your CORS policy correctly, and troubleshoot if things don’t work as you expect.</span></span>

<span data-ttu-id="020ae-210">Specifikace CORS zavádí několik nových hlavičky HTTP, které povolení žádostí napříč zdroji.</span><span class="sxs-lookup"><span data-stu-id="020ae-210">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="020ae-211">Pokud prohlížeč podporuje CORS, nastaví tato záhlaví automaticky pro požadavky cross-origin; nemusíte provádět žádné zvláštní v kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="020ae-211">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don’t need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="020ae-212">Tady je příklad žádost nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="020ae-212">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="020ae-213">Záhlaví "Původ" dává domény, lokality, která odeslala žádost:</span><span class="sxs-lookup"><span data-stu-id="020ae-213">The "Origin" header gives the domain of the site that is making the request:</span></span>

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

<span data-ttu-id="020ae-214">Pokud server umožňuje žádost, nastaví hlavičky Access-Control-Allow-Origin v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="020ae-214">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="020ae-215">Hodnotu této hlavičky buď odpovídá počátek hlavička ze žádosti, nebo je hodnota zástupný znak "*", což znamená, že je povoleno jakýkoli původ:</span><span class="sxs-lookup"><span data-stu-id="020ae-215">The value of this header either matches the Origin header from the request, or is the wildcard value "*", meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="020ae-216">Pokud odpověď nezahrnuje hlavičky Access-Control-Allow-Origin, požadavek AJAX se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="020ae-216">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="020ae-217">V prohlížeči přímo, neumožňuje žádosti.</span><span class="sxs-lookup"><span data-stu-id="020ae-217">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="020ae-218">I když server vrátí úspěšné odpovědi, prohlížeč není zpřístupnit odpověď do klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="020ae-218">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="020ae-219">Předběžných požadavků</span><span class="sxs-lookup"><span data-stu-id="020ae-219">Preflight Requests</span></span>

<span data-ttu-id="020ae-220">U některých požadavků CORS prohlížeč odešle požadavek další, nazývá "předběžný požadavek", než odešle skutečné požadavek na prostředek.</span><span class="sxs-lookup"><span data-stu-id="020ae-220">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="020ae-221">V prohlížeči můžete přeskočit předběžný požadavek, pokud jsou splněny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="020ae-221">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="020ae-222">Metoda požadavku je GET, HEAD nebo POST, a</span><span class="sxs-lookup"><span data-stu-id="020ae-222">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="020ae-223">Aplikace není nastavena všechny hlavičky požadavku než přijmout, Accept-Language, jazyk obsahu Content-Type nebo poslední událost ID, a</span><span class="sxs-lookup"><span data-stu-id="020ae-223">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="020ae-224">Hlavička Content-Type (Pokud nastavit) je jedním z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="020ae-224">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="020ae-225">Application/x--www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="020ae-225">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="020ae-226">multipart/formulář data</span><span class="sxs-lookup"><span data-stu-id="020ae-226">multipart/form-data</span></span>

  * <span data-ttu-id="020ae-227">text/plain</span><span class="sxs-lookup"><span data-stu-id="020ae-227">text/plain</span></span>

<span data-ttu-id="020ae-228">Pravidlo o hlavičky požadavku platí pro hlavičky, které aplikace nastaví voláním setRequestHeader XMLHttpRequest objektu.</span><span class="sxs-lookup"><span data-stu-id="020ae-228">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="020ae-229">(Specifikace CORS volá tyto "Autor hlavičky žádosti"). Pravidlo se nevztahuje na hlavičky, které můžete nastavit v prohlížeči, třeba uživatelského agenta, hostitele nebo Content-Length.</span><span class="sxs-lookup"><span data-stu-id="020ae-229">(The CORS specification calls these "author request headers".) The rule does not apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="020ae-230">Tady je příklad předběžný požadavek:</span><span class="sxs-lookup"><span data-stu-id="020ae-230">Here is an example of a preflight request:</span></span>

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

<span data-ttu-id="020ae-231">Žádost o předběžné letu používá metodu HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="020ae-231">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="020ae-232">Obsahuje dva speciální hlavičky:</span><span class="sxs-lookup"><span data-stu-id="020ae-232">It includes two special headers:</span></span>

* <span data-ttu-id="020ae-233">Access-Control-Request-Method: Metodu protokolu HTTP, který se použije pro skutečný požadavek.</span><span class="sxs-lookup"><span data-stu-id="020ae-233">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="020ae-234">Access-Control-Request-Headers: Seznam hlaviček požadavků, které aplikace nastavena na skutečnou žádost.</span><span class="sxs-lookup"><span data-stu-id="020ae-234">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="020ae-235">(Znovu, nezahrnuje hlavičky, které nastaví prohlížeče.)</span><span class="sxs-lookup"><span data-stu-id="020ae-235">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="020ae-236">Zde je odpověď příklad, za předpokladu, že server umožňuje žádost:</span><span class="sxs-lookup"><span data-stu-id="020ae-236">Here is an example response, assuming that the server allows the request:</span></span>

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

<span data-ttu-id="020ae-237">Odpověď obsahuje hlavičku přístup – ovládací prvek-Allow-Methods, který obsahuje seznam povolených metod a volitelně hlavičky Access-Control-povolit-Headers, kde jsou uvedeny povolené hlavičky.</span><span class="sxs-lookup"><span data-stu-id="020ae-237">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="020ae-238">Pokud předběžný požadavek úspěšné, prohlížeč odesílá skutečné požadavku, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="020ae-238">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
