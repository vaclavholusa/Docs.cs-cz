---
title: Povolení žádostí napříč zdroji (CORS) v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak CORS jako standard pro povolení nebo odmítnutí žádostí napříč zdroji v aplikaci ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 05/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cors
ms.openlocfilehash: 3c5d0840426c7ed52353a7832a1a1959027121de
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
ms.locfileid: "30077545"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="15e04-103">Povolení žádostí napříč zdroji (CORS) v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15e04-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="15e04-104">Podle [Karel Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), a [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="15e04-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="15e04-105">Zabezpečení prohlížeče brání provedení požadavky AJAX do jiné domény na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="15e04-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="15e04-106">Toto omezení se nazývá *zásady stejného původu*a zabrání škodlivé weby čtení citlivá data z jiné lokality.</span><span class="sxs-lookup"><span data-stu-id="15e04-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="15e04-107">Ale v některých případech můžete chtít nechat ostatních lokalit, zkontrolujte žádostí napříč zdroji do webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="15e04-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="15e04-108">[Mezi sdílení prostředků původu](http://www.w3.org/TR/cors/) (CORS) je standard W3C, který umožňuje serveru zmírnit zásady stejného původu.</span><span class="sxs-lookup"><span data-stu-id="15e04-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="15e04-109">Pomocí CORS, server explicitně povolit některé žádostí napříč zdroji při odmítnutí ostatní.</span><span class="sxs-lookup"><span data-stu-id="15e04-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="15e04-110">CORS, jako je bezpečnější a flexibilnější než dřívější techniky [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="15e04-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="15e04-111">Toto téma ukazuje postup povolení CORS v aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="15e04-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="15e04-112">Co je "stejné původ"?</span><span class="sxs-lookup"><span data-stu-id="15e04-112">What is "same origin"?</span></span>

<span data-ttu-id="15e04-113">Dvou adres URL mají stejný původ v případě, že mají stejné schémata, hostitele a porty.</span><span class="sxs-lookup"><span data-stu-id="15e04-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="15e04-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="15e04-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="15e04-115">Tyto dvě adresy URL mají stejný původ:</span><span class="sxs-lookup"><span data-stu-id="15e04-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="15e04-116">Tyto adresy URL mít různého původu než předchozí dva:</span><span class="sxs-lookup"><span data-stu-id="15e04-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="15e04-117">`http://example.net` -Jiné domény</span><span class="sxs-lookup"><span data-stu-id="15e04-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="15e04-118">`http://www.example.com/foo.html` -Různých subdomény</span><span class="sxs-lookup"><span data-stu-id="15e04-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="15e04-119">`https://example.com/foo.html` -Jiné schéma</span><span class="sxs-lookup"><span data-stu-id="15e04-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="15e04-120">`http://example.com:9000/foo.html` -Jiný port</span><span class="sxs-lookup"><span data-stu-id="15e04-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="15e04-121">Internet Explorer nepovažuje port při porovnávání zdroje.</span><span class="sxs-lookup"><span data-stu-id="15e04-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="setting-up-cors"></a><span data-ttu-id="15e04-122">Nastavení CORS</span><span class="sxs-lookup"><span data-stu-id="15e04-122">Setting up CORS</span></span>

<span data-ttu-id="15e04-123">K nastavení CORS pro vaši aplikaci přidat `Microsoft.AspNetCore.Cors` balíčku do projektu.</span><span class="sxs-lookup"><span data-stu-id="15e04-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

<span data-ttu-id="15e04-124">Přidání služeb CORS v souboru Startup.cs:</span><span class="sxs-lookup"><span data-stu-id="15e04-124">Add the CORS services in Startup.cs:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="15e04-125">Povolení CORS s middlewaru.</span><span class="sxs-lookup"><span data-stu-id="15e04-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="15e04-126">Chcete-li přidat CORS pro celou aplikaci CORS middleware do kanálu vaší žádosti o pomocí `UseCors` metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="15e04-126">To enable CORS for your entire application add the CORS middleware to your request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="15e04-127">Všimněte si, že CORS middleware musí předcházet žádné koncové body definované ve vaší aplikaci, která chcete podporovat žádostí napříč zdroji (např.)</span><span class="sxs-lookup"><span data-stu-id="15e04-127">Note that the CORS middleware must precede any defined endpoints in your app that you want to support cross-origin requests (ex.</span></span> <span data-ttu-id="15e04-128">Před voláním jakékoli `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="15e04-128">before any call to `UseMvc`).</span></span>

<span data-ttu-id="15e04-129">Při přidávání pomocí middleware CORS můžete zadat zásady cross-origin `CorsPolicyBuilder` třídy.</span><span class="sxs-lookup"><span data-stu-id="15e04-129">You can specify a cross-origin policy when adding the CORS middleware using the `CorsPolicyBuilder` class.</span></span> <span data-ttu-id="15e04-130">Chcete-li to provést dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="15e04-130">There are two ways to do this.</span></span> <span data-ttu-id="15e04-131">První je volání UseCors s lambda:</span><span class="sxs-lookup"><span data-stu-id="15e04-131">The first is to call UseCors with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="15e04-132">**Poznámka:** bez koncové lomítko musíte zadat adresu URL (`/`).</span><span class="sxs-lookup"><span data-stu-id="15e04-132">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="15e04-133">Pokud adresu URL ukončí s `/`, vrátí porovnání `false` , které budou vráceny žádné záhlaví.</span><span class="sxs-lookup"><span data-stu-id="15e04-133">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="15e04-134">Argument lambda trvá `CorsPolicyBuilder` objektu.</span><span class="sxs-lookup"><span data-stu-id="15e04-134">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="15e04-135">Seznam najdete [možnosti konfigurace](#cors-policy-options) dál v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="15e04-135">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="15e04-136">V tomto příkladu zásada umožňuje žádostí napříč zdroji z `http://example.com` a žádné jiné zdroje.</span><span class="sxs-lookup"><span data-stu-id="15e04-136">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="15e04-137">Všimněte si, že CorsPolicyBuilder obsahuje rozhraní fluent API, takže můžete řetězu volání metod:</span><span class="sxs-lookup"><span data-stu-id="15e04-137">Note that CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="15e04-138">Druhý postup je definovat jeden nebo více s názvem zásady CORS, a potom vyberte zásady podle názvu v době běhu.</span><span class="sxs-lookup"><span data-stu-id="15e04-138">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="15e04-139">Tento příklad přidá zásadu CORS s názvem "AllowSpecificOrigin".</span><span class="sxs-lookup"><span data-stu-id="15e04-139">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="15e04-140">Vyberte zásadu, předat název, který má `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="15e04-140">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="15e04-141">Povolení CORS v MVC</span><span class="sxs-lookup"><span data-stu-id="15e04-141">Enabling CORS in MVC</span></span>

<span data-ttu-id="15e04-142">Případně můžete MVC použít konkrétní CORS na každou akci, na jeden kontroler nebo globálně pro všechny řadiče.</span><span class="sxs-lookup"><span data-stu-id="15e04-142">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="15e04-143">Při použití MVC k povolení sdílení CORS se používají stejné CORS služby, ale není CORS middleware.</span><span class="sxs-lookup"><span data-stu-id="15e04-143">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="15e04-144">Na každou akci</span><span class="sxs-lookup"><span data-stu-id="15e04-144">Per action</span></span>

<span data-ttu-id="15e04-145">Zadejte přidat zásadu CORS pro určité akci `[EnableCors]` atribut akce.</span><span class="sxs-lookup"><span data-stu-id="15e04-145">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="15e04-146">Zadejte název zásady.</span><span class="sxs-lookup"><span data-stu-id="15e04-146">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="15e04-147">Na jeden kontroler</span><span class="sxs-lookup"><span data-stu-id="15e04-147">Per controller</span></span>

<span data-ttu-id="15e04-148">Zadejte zásady CORS pro konkrétní řadič přidat `[EnableCors]` atributu do třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="15e04-148">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="15e04-149">Zadejte název zásady.</span><span class="sxs-lookup"><span data-stu-id="15e04-149">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="15e04-150">Globálně</span><span class="sxs-lookup"><span data-stu-id="15e04-150">Globally</span></span>

<span data-ttu-id="15e04-151">Můžete povolit CORS globálně pro všechny řadiče přidáním `CorsAuthorizationFilterFactory` filtr do kolekce globálních filtrů:</span><span class="sxs-lookup"><span data-stu-id="15e04-151">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="15e04-152">Pořadí priorit je: akci, kontroler, globální.</span><span class="sxs-lookup"><span data-stu-id="15e04-152">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="15e04-153">Zásady na úrovni akce mají přednost před zásady na úrovni kontroleru a zásady na úrovni kontroleru mají přednost před globální zásady.</span><span class="sxs-lookup"><span data-stu-id="15e04-153">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="15e04-154">Zakázat CORS</span><span class="sxs-lookup"><span data-stu-id="15e04-154">Disable CORS</span></span>

<span data-ttu-id="15e04-155">Chcete-li zakázat CORS pro kontroler nebo akce, použijte `[DisableCors]` atribut.</span><span class="sxs-lookup"><span data-stu-id="15e04-155">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="15e04-156">Možnosti zásad CORS</span><span class="sxs-lookup"><span data-stu-id="15e04-156">CORS policy options</span></span>

<span data-ttu-id="15e04-157">Tato část popisuje různé možnosti, které můžete nastavit v zásadu CORS.</span><span class="sxs-lookup"><span data-stu-id="15e04-157">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="15e04-158">Nastavit povolené zdroje</span><span class="sxs-lookup"><span data-stu-id="15e04-158">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="15e04-159">Nastavení povolených metod HTTP</span><span class="sxs-lookup"><span data-stu-id="15e04-159">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="15e04-160">Nastavit hlavičky povolených požadavků</span><span class="sxs-lookup"><span data-stu-id="15e04-160">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="15e04-161">Nastavit hlavičky zveřejněné odpovědi</span><span class="sxs-lookup"><span data-stu-id="15e04-161">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="15e04-162">Přihlašovací údaje v žádostí napříč zdroji</span><span class="sxs-lookup"><span data-stu-id="15e04-162">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="15e04-163">Nastavte hodnotu doby předběžných vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="15e04-163">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="15e04-164">Pro některé možnosti může být užitečné číst [funguje jak CORS](#how-cors-works) první.</span><span class="sxs-lookup"><span data-stu-id="15e04-164">For some options it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="15e04-165">Nastavit povolené zdroje</span><span class="sxs-lookup"><span data-stu-id="15e04-165">Set the allowed origins</span></span>

<span data-ttu-id="15e04-166">Chcete-li povolit jeden nebo více konkrétních zdroje:</span><span class="sxs-lookup"><span data-stu-id="15e04-166">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="15e04-167">Chcete-li povolit všechny původy:</span><span class="sxs-lookup"><span data-stu-id="15e04-167">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="15e04-168">Pečlivě zvažte, před povolením požadavky od jakýkoli původ.</span><span class="sxs-lookup"><span data-stu-id="15e04-168">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="15e04-169">Znamená to, že názvy všech webu provádět volání AJAX k vašemu rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="15e04-169">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="15e04-170">Nastavení povolených metod HTTP</span><span class="sxs-lookup"><span data-stu-id="15e04-170">Set the allowed HTTP methods</span></span>

<span data-ttu-id="15e04-171">Chcete-li povolit všechny metody HTTP:</span><span class="sxs-lookup"><span data-stu-id="15e04-171">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="15e04-172">To ovlivní letu předběžné požadavky a záhlaví přístupu – ovládací prvek-Allow-Methods.</span><span class="sxs-lookup"><span data-stu-id="15e04-172">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="15e04-173">Nastavit hlavičky povolených požadavků</span><span class="sxs-lookup"><span data-stu-id="15e04-173">Set the allowed request headers</span></span>

<span data-ttu-id="15e04-174">Předběžný požadavek CORS může obsahovat hlavičku Access-Control-Request-Headers výpis hlavičky protokolu HTTP, které jsou nastaveny aplikací (takzvaný "vytvářet hlavičky žádosti").</span><span class="sxs-lookup"><span data-stu-id="15e04-174">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="15e04-175">Pro konkrétní hlavičky seznamu povolených IP adres:</span><span class="sxs-lookup"><span data-stu-id="15e04-175">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="15e04-176">Umožňuje vytvářet všechny hlavičky žádosti:</span><span class="sxs-lookup"><span data-stu-id="15e04-176">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="15e04-177">Nejsou prohlížeče zcela v souladu v tom, jak nastavují Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="15e04-177">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="15e04-178">Pokud nastavíte hlavičky k ničemu jiné než "\*", by měla obsahovat alespoň "přijmout", "content-type" a "Původ" a jakékoli vlastní hlavičky, které chcete podporovat.</span><span class="sxs-lookup"><span data-stu-id="15e04-178">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="15e04-179">Nastavit hlavičky zveřejněné odpovědi</span><span class="sxs-lookup"><span data-stu-id="15e04-179">Set the exposed response headers</span></span>

<span data-ttu-id="15e04-180">Ve výchozím prohlížeči nezveřejňuje všechny hlavičky odpovědi do aplikace.</span><span class="sxs-lookup"><span data-stu-id="15e04-180">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="15e04-181">(Viz [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Hlavičky odpovědi, které jsou k dispozici ve výchozím nastavení jsou:</span><span class="sxs-lookup"><span data-stu-id="15e04-181">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="15e04-182">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="15e04-182">Cache-Control</span></span>

* <span data-ttu-id="15e04-183">Obsah – jazyk</span><span class="sxs-lookup"><span data-stu-id="15e04-183">Content-Language</span></span>

* <span data-ttu-id="15e04-184">Typ obsahu</span><span class="sxs-lookup"><span data-stu-id="15e04-184">Content-Type</span></span>

* <span data-ttu-id="15e04-185">Vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="15e04-185">Expires</span></span>

* <span data-ttu-id="15e04-186">Poslední úpravy</span><span class="sxs-lookup"><span data-stu-id="15e04-186">Last-Modified</span></span>

* <span data-ttu-id="15e04-187">Direktiva pragma</span><span class="sxs-lookup"><span data-stu-id="15e04-187">Pragma</span></span>

<span data-ttu-id="15e04-188">Specifikace CORS volá tyto *hlavičky odpovědi jednoduché*.</span><span class="sxs-lookup"><span data-stu-id="15e04-188">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="15e04-189">Pro zpřístupnění jiných hlavičky pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="15e04-189">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="15e04-190">Přihlašovací údaje v žádostí napříč zdroji</span><span class="sxs-lookup"><span data-stu-id="15e04-190">Credentials in cross-origin requests</span></span>

<span data-ttu-id="15e04-191">Přihlašovací údaje vyžadují zvláštní zpracování v požadavek CORS.</span><span class="sxs-lookup"><span data-stu-id="15e04-191">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="15e04-192">Ve výchozím prohlížeči neodešle všechny přihlašovací údaje s žádostí napříč zdroji.</span><span class="sxs-lookup"><span data-stu-id="15e04-192">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="15e04-193">Přihlašovací údaje zahrnují soubory cookie, jakož i schémat ověřování protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="15e04-193">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="15e04-194">K odeslání pověření s žádostí napříč zdroji, musí klient nastavit XMLHttpRequest.withCredentials na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="15e04-194">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="15e04-195">Pomocí rozhraní XMLHttpRequest přímo:</span><span class="sxs-lookup"><span data-stu-id="15e04-195">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="15e04-196">V jQuery:</span><span class="sxs-lookup"><span data-stu-id="15e04-196">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="15e04-197">Kromě toho serveru musí umožňovat přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="15e04-197">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="15e04-198">Chcete-li povolit cross-origin přihlašovací údaje:</span><span class="sxs-lookup"><span data-stu-id="15e04-198">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="15e04-199">Odpověď HTTP bude teď obsahovat přístup – ovládací prvek-Allow-Credentials záhlaví, která sděluje prohlížeči, že server umožňuje přihlašovací údaje pro žádost o nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="15e04-199">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="15e04-200">Pokud prohlížeč odesílá přihlašovací údaje, ale odpověď neobsahuje platný přístup – ovládací prvek-Allow-Credentials záhlaví, prohlížeče nebude vystavit odpověď na žádost a vyvolá chybu požadavek AJAX.</span><span class="sxs-lookup"><span data-stu-id="15e04-200">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="15e04-201">Buďte opatrní při povolení cross-origin přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="15e04-201">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="15e04-202">Web v jiné doméně odeslat do aplikace jménem uživatele bez vědomí uživatele pověření přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="15e04-202">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="15e04-203">Specifikace CORS také stavy tohoto nastavení počátky k "\*" (všechny původy) je neplatný Pokud `Access-Control-Allow-Credentials` záhlaví nachází.</span><span class="sxs-lookup"><span data-stu-id="15e04-203">The CORS specification also states that setting origins to "\*" (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="15e04-204">Nastavte hodnotu doby předběžných vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="15e04-204">Set the preflight expiration time</span></span>

<span data-ttu-id="15e04-205">Záhlaví přístupu – ovládací prvek-Max-Age Určuje, jak dlouho může do mezipaměti odpovědi na předběžný požadavek.</span><span class="sxs-lookup"><span data-stu-id="15e04-205">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="15e04-206">Chcete-li nastavit tuto hlavičku:</span><span class="sxs-lookup"><span data-stu-id="15e04-206">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="15e04-207">Jak funguje CORS</span><span class="sxs-lookup"><span data-stu-id="15e04-207">How CORS works</span></span>

<span data-ttu-id="15e04-208">Tato část popisuje, co se stane, že v požadavku CORS na úrovni zpráv protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="15e04-208">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="15e04-209">Je důležité pochopit, jak CORS funguje tak, aby zásada CORS můžete správně nakonfigurovaná a troubleshooted když dojde k neočekávané chování.</span><span class="sxs-lookup"><span data-stu-id="15e04-209">It's important to understand how CORS works so that the CORS policy can be configured correctly and troubleshooted when unexpected behaviors occur.</span></span>

<span data-ttu-id="15e04-210">Specifikace CORS zavádí několik nových hlavičky HTTP, které povolení žádostí napříč zdroji.</span><span class="sxs-lookup"><span data-stu-id="15e04-210">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="15e04-211">Pokud prohlížeč podporuje CORS, nastaví tato záhlaví automaticky pro požadavky cross-origin.</span><span class="sxs-lookup"><span data-stu-id="15e04-211">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="15e04-212">Vlastní kód JavaScript není vyžadován k povolení sdílení CORS.</span><span class="sxs-lookup"><span data-stu-id="15e04-212">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="15e04-213">Tady je příklad žádost nepůvodního zdroje.</span><span class="sxs-lookup"><span data-stu-id="15e04-213">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="15e04-214">`Origin` Záhlaví poskytuje domény, lokality, která odeslala žádost:</span><span class="sxs-lookup"><span data-stu-id="15e04-214">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="15e04-215">Pokud server umožňuje žádost, nastaví hlavičky Access-Control-Allow-Origin v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="15e04-215">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="15e04-216">Hodnotu této hlavičky buď odpovídá počátek hlavička ze žádosti, nebo je hodnota zástupný znak "\*", což znamená, že je povoleno jakýkoli původ:</span><span class="sxs-lookup"><span data-stu-id="15e04-216">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="15e04-217">Pokud odpověď neobsahuje hlavičky Access-Control-Allow-Origin, požadavek AJAX se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="15e04-217">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="15e04-218">V prohlížeči přímo, neumožňuje žádosti.</span><span class="sxs-lookup"><span data-stu-id="15e04-218">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="15e04-219">I když server vrátí úspěšné odpovědi, prohlížeč nepodporuje zpřístupnit odpověď do klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="15e04-219">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="15e04-220">Předběžných požadavků</span><span class="sxs-lookup"><span data-stu-id="15e04-220">Preflight Requests</span></span>

<span data-ttu-id="15e04-221">U některých požadavků CORS prohlížeč odešle požadavek další, nazývá "předběžný požadavek", než odešle skutečné požadavek na prostředek.</span><span class="sxs-lookup"><span data-stu-id="15e04-221">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="15e04-222">V prohlížeči můžete přeskočit předběžný požadavek, pokud jsou splněny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="15e04-222">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="15e04-223">Metoda požadavku je GET, HEAD nebo POST, a</span><span class="sxs-lookup"><span data-stu-id="15e04-223">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="15e04-224">Aplikace není nastavena všechny hlavičky požadavku než přijmout, Accept-Language, jazyk obsahu Content-Type nebo poslední událost ID, a</span><span class="sxs-lookup"><span data-stu-id="15e04-224">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="15e04-225">Hlavička Content-Type (Pokud nastavit) je jedním z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="15e04-225">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="15e04-226">Application/x--www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="15e04-226">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="15e04-227">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="15e04-227">multipart/form-data</span></span>

  * <span data-ttu-id="15e04-228">text/plain</span><span class="sxs-lookup"><span data-stu-id="15e04-228">text/plain</span></span>

<span data-ttu-id="15e04-229">Pravidlo o hlavičky požadavku platí pro hlavičky, které aplikace nastaví voláním setRequestHeader XMLHttpRequest objektu.</span><span class="sxs-lookup"><span data-stu-id="15e04-229">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="15e04-230">(Specifikace CORS volá tyto "Autor hlavičky žádosti"). Pravidlo se nevztahuje na hlavičky, které můžete nastavit v prohlížeči, třeba uživatelského agenta, hostitele nebo Content-Length.</span><span class="sxs-lookup"><span data-stu-id="15e04-230">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="15e04-231">Tady je příklad předběžný požadavek:</span><span class="sxs-lookup"><span data-stu-id="15e04-231">Here is an example of a preflight request:</span></span>

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

<span data-ttu-id="15e04-232">Žádost o předběžné letu používá metodu HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="15e04-232">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="15e04-233">Obsahuje dva speciální hlavičky:</span><span class="sxs-lookup"><span data-stu-id="15e04-233">It includes two special headers:</span></span>

* <span data-ttu-id="15e04-234">Access-Control-Request-Method: Metodu protokolu HTTP, který se použije pro skutečný požadavek.</span><span class="sxs-lookup"><span data-stu-id="15e04-234">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="15e04-235">Access-Control-Request-Headers: Seznam hlaviček požadavků, které aplikace nastavena na skutečnou žádost.</span><span class="sxs-lookup"><span data-stu-id="15e04-235">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="15e04-236">(Znovu to neobsahuje hlavičky, které nastaví prohlížeče.)</span><span class="sxs-lookup"><span data-stu-id="15e04-236">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="15e04-237">Zde je odpověď příklad, za předpokladu, že server umožňuje žádost:</span><span class="sxs-lookup"><span data-stu-id="15e04-237">Here is an example response, assuming that the server allows the request:</span></span>

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

<span data-ttu-id="15e04-238">Odpověď obsahuje hlavičku přístup – ovládací prvek-Allow-Methods, který obsahuje seznam povolených metod a volitelně hlavičky Access-Control-povolit-Headers, kde jsou uvedeny povolené hlavičky.</span><span class="sxs-lookup"><span data-stu-id="15e04-238">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="15e04-239">Pokud předběžný požadavek úspěšné, prohlížeč odesílá skutečné požadavku, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="15e04-239">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
