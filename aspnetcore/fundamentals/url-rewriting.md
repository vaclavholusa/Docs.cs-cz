---
title: "Middleware v ASP.NET Core přepisování adres URL"
author: guardrex
description: "Další informace o adrese URL přepisování a přesměrování s Middleware přepisování adresy URL v aplikacích ASP.NET Core."
keywords: "ASP.NET Core, přepisování adres URL, adresa URL přesměrování, adresa URL přesměrování, middleware, apache_mod přepisování adres URL"
ms.author: riande
manager: wpickett
ms.date: 08/17/2017
ms.topic: article
ms.assetid: e6130638-c410-4161-9921-b658ce988bd1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/url-rewriting
ms.openlocfilehash: dde0b5673c9885db2fecbb24b384752e5ddf70eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="78efd-104">Middleware v ASP.NET Core přepisování adres URL</span><span class="sxs-lookup"><span data-stu-id="78efd-104">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="78efd-105">Podle [Luke Latham](https://github.com/guardrex) a [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="78efd-105">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="78efd-106">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="78efd-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="78efd-107">Přepisování adres URL je v rámci úprava požadavek adresy URL založené na jeden nebo více předdefinovaných pravidel.</span><span class="sxs-lookup"><span data-stu-id="78efd-107">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="78efd-108">Přepisování adres URL vytváří abstrakci mezi umístění prostředků a jejich adres, takže umístění a adresy nejsou propojené úzce.</span><span class="sxs-lookup"><span data-stu-id="78efd-108">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="78efd-109">Existuje několik situací, kdy je vhodné přepisování adres URL:</span><span class="sxs-lookup"><span data-stu-id="78efd-109">There are several scenarios where URL rewriting is valuable:</span></span>
* <span data-ttu-id="78efd-110">Přesunutí nebo výměna prostředky serveru dočasně nebo trvale při zachování stabilní lokátory pro tyto prostředky</span><span class="sxs-lookup"><span data-stu-id="78efd-110">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources</span></span>
* <span data-ttu-id="78efd-111">Rozdělení přes různé aplikace nebo přes oblasti jednu aplikaci se zpracováním požadavků</span><span class="sxs-lookup"><span data-stu-id="78efd-111">Splitting request processing across different apps or across areas of one app</span></span>
* <span data-ttu-id="78efd-112">Odebrání, přidání nebo změna uspořádání segmenty adres URL na příchozí požadavky</span><span class="sxs-lookup"><span data-stu-id="78efd-112">Removing, adding, or reorganizing URL segments on incoming requests</span></span>
* <span data-ttu-id="78efd-113">Optimalizace veřejné adresy URL pro hledání modul optimalizace (SEO)</span><span class="sxs-lookup"><span data-stu-id="78efd-113">Optimizing public URLs for Search Engine Optimization (SEO)</span></span>
* <span data-ttu-id="78efd-114">Umožňuje použití popisný veřejné adresy URL pro přehledné předpovědi obsah, který bude najít klepnutím na odkaz</span><span class="sxs-lookup"><span data-stu-id="78efd-114">Permitting the use of friendly public URLs to help people predict the content they will find by following a link</span></span>
* <span data-ttu-id="78efd-115">Přesměrování nezabezpečené požadavky na zabezpečení koncových bodů</span><span class="sxs-lookup"><span data-stu-id="78efd-115">Redirecting insecure requests to secure endpoints</span></span>
* <span data-ttu-id="78efd-116">Brání hotlinking bitové kopie</span><span class="sxs-lookup"><span data-stu-id="78efd-116">Preventing image hotlinking</span></span>

<span data-ttu-id="78efd-117">Můžete definovat pravidla pro změny adresy URL několika způsoby, včetně regex, Apache mod_rewrite modulu pravidla a pravidla modul služby IIS a pomocí vlastního pravidla logiku.</span><span class="sxs-lookup"><span data-stu-id="78efd-117">You can define rules for changing the URL in several ways, including regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="78efd-118">Toto téma představuje přepisování adres URL s pokyny, jak používat Middleware přepisování adresy URL v aplikacích ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="78efd-118">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="78efd-119">Přepisování adres URL může snížit výkon aplikace.</span><span class="sxs-lookup"><span data-stu-id="78efd-119">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="78efd-120">Kde je to vhodné, měli byste omezit počet a složitost pravidel.</span><span class="sxs-lookup"><span data-stu-id="78efd-120">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="78efd-121">Adresa URL přesměrování a adresy URL přepisování</span><span class="sxs-lookup"><span data-stu-id="78efd-121">URL redirect and URL rewrite</span></span>
<span data-ttu-id="78efd-122">Rozdíl ve slov mezi *adresa URL přesměrování* a *přepisování adres URL* se může zdát jemně na první, ale nemá důležité zvážit celkové důsledky pro poskytování prostředků klientům.</span><span class="sxs-lookup"><span data-stu-id="78efd-122">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="78efd-123">Middleware přepisování ASP.NET Core adresa URL je schopen splňuje potřeby pro obě.</span><span class="sxs-lookup"><span data-stu-id="78efd-123">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="78efd-124">A *adresa URL přesměrování* je operace na straně klienta, kde je pro přístup k prostředkům na jiné adrese pokyn klientovi.</span><span class="sxs-lookup"><span data-stu-id="78efd-124">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="78efd-125">To vyžaduje, aby mít odezvu na server a adresa URL pro přesměrování vrácen do klienta se zobrazí v panelu Adresa prohlížeče, když klient podá nová žádost o prostředek.</span><span class="sxs-lookup"><span data-stu-id="78efd-125">This requires a round-trip to the server, and the redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> <span data-ttu-id="78efd-126">Pokud `/resource` je *přesměrováno* k `/different-resource`, požadavky klientů `/resource`, a server odpoví, že klient musí získat prostředek na `/different-resource` s stav kód označující, že je přesměrování dočasné nebo trvalé.</span><span class="sxs-lookup"><span data-stu-id="78efd-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`, and the server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="78efd-127">Klient spustí nová žádost o prostředek na adrese URL pro přesměrování.</span><span class="sxs-lookup"><span data-stu-id="78efd-127">The client executes a new request for the resource at the redirect URL.</span></span>

![Koncový bod služby WebAPI dočasně změnil z verze 1 (v1) na verze 2 (v2) na serveru.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="78efd-133">Při přesměrování požadavků na jinou adresu URL, které označuje, zda přesměrování trvalé nebo dočasné.</span><span class="sxs-lookup"><span data-stu-id="78efd-133">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="78efd-134">301 (trvale přesunut) kód stavu se používá kde prostředek má novou, trvalé URL a chcete do pokyn klientovi, že všechny budoucí požadavky pro prostředek by měl používat nové adrese URL.</span><span class="sxs-lookup"><span data-stu-id="78efd-134">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="78efd-135">*Klient může odpověď do mezipaměti, když je obdržena 301 stavový kód.*</span><span class="sxs-lookup"><span data-stu-id="78efd-135">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="78efd-136">302 stavový kód (Found) se používá, kde přesměrování je dočasný nebo obecně subjektu Pokud chcete změnit, tak, aby klient by neměl uložit a opakovaně používat v budoucnosti adresa URL pro přesměrování.</span><span class="sxs-lookup"><span data-stu-id="78efd-136">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="78efd-137">Další informace najdete v tématu [dokumentu RFC 2616: definice stavových kódů](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="78efd-137">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="78efd-138">A *přepisování adres URL* je operaci na straně serveru zadejte prostředek z adresy jiného prostředku.</span><span class="sxs-lookup"><span data-stu-id="78efd-138">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="78efd-139">Přepisování adresu URL nevyžaduje round-trip k serveru.</span><span class="sxs-lookup"><span data-stu-id="78efd-139">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="78efd-140">Rewritten adresa URL není vrácen do klienta a nezobrazí se v panelu Adresa prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="78efd-140">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="78efd-141">Když `/resource` je *přepsaná* k `/different-resource`, požadavky klientů `/resource`a server *interně* načte prostředek na `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="78efd-141">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="78efd-142">I když může být klient schopný načíst prostředek na adresu URL rewritten, klienta nebude informováni zda prostředek existuje na rewritten adrese URL, když vytvoří požadavku a obdrží odpověď.</span><span class="sxs-lookup"><span data-stu-id="78efd-142">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Koncový bod služby WebAPI byl změněn z verze 1 (v1) verze 2 (v2) na serveru.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="78efd-147">Adresa URL třeba přepisovat ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="78efd-147">URL rewriting sample app</span></span>
<span data-ttu-id="78efd-148">Můžete prozkoumat funkce middlewaru přepisování adresy URL se [URL třeba přepisovat ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span><span class="sxs-lookup"><span data-stu-id="78efd-148">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span></span> <span data-ttu-id="78efd-149">Aplikace používá přepisování a přesměrování pravidla a zobrazuje přepsaná nebo přesměrované adresu URL.</span><span class="sxs-lookup"><span data-stu-id="78efd-149">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="78efd-150">Kdy použít adresu URL přepisování middlewaru</span><span class="sxs-lookup"><span data-stu-id="78efd-150">When to use URL Rewriting Middleware</span></span>
<span data-ttu-id="78efd-151">Použijte adresu URL přepisování Middleware, pokud nelze použít [modul přepisování adres URL](https://www.iis.net/downloads/microsoft/url-rewrite) službou IIS v systému Windows Server, [Apache mod_rewrite modulu](https://httpd.apache.org/docs/2.4/rewrite/) na serveru Apache [přepisování adres URL na Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), nebo aplikace je hostovaná na [HTTP.sys serveru](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="78efd-151">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="78efd-152">Hlavní důvody pro použití třeba přepisovat adresy URL na serveru technologie v IIS, Apache nebo Nginx je, že middleware nepodporuje úplné funkce tyto moduly a výkon middleware pravděpodobně nebude odpovídat moduly.</span><span class="sxs-lookup"><span data-stu-id="78efd-152">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="78efd-153">Existují však některé funkce serveru moduly, které nepodporují s projekty ASP.NET Core, jako `IsFile` a `IsDirectory` omezení modul přepisování služby IIS.</span><span class="sxs-lookup"><span data-stu-id="78efd-153">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="78efd-154">V těchto scénářích použijte místo toho middleware.</span><span class="sxs-lookup"><span data-stu-id="78efd-154">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="78efd-155">Balíček</span><span class="sxs-lookup"><span data-stu-id="78efd-155">Package</span></span>
<span data-ttu-id="78efd-156">Zahrnout middleware projekt, přidejte odkaz na [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="78efd-156">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="78efd-157">Tato funkce je k dispozici pro aplikace, které cílí ASP.NET Core 1.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="78efd-157">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="78efd-158">Rozšíření a možnosti</span><span class="sxs-lookup"><span data-stu-id="78efd-158">Extension and options</span></span>
<span data-ttu-id="78efd-159">Vytvoření vaší přepisování adres URL a přesměrování pravidla tak, že vytvoříte instanci `RewriteOptions` se rozšiřující metody pro každou z pravidel.</span><span class="sxs-lookup"><span data-stu-id="78efd-159">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="78efd-160">Zřetězené více pravidel v pořadí, které chcete je zpracovat.</span><span class="sxs-lookup"><span data-stu-id="78efd-160">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="78efd-161">`RewriteOptions` Jsou předané do Middleware přepisování adresy URL se přidá do kanálu požadavek s `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="78efd-161">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="78efd-162">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="78efd-162">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="78efd-163">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="78efd-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a><span data-ttu-id="78efd-164">Adresa URL přesměrování</span><span class="sxs-lookup"><span data-stu-id="78efd-164">URL redirect</span></span>
<span data-ttu-id="78efd-165">Použití `AddRedirect` pro přesměrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="78efd-165">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="78efd-166">První parametr obsahuje vaše regulární výraz k porovnání na cestě příchozí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="78efd-166">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="78efd-167">Druhý parametr je náhradní řetězec.</span><span class="sxs-lookup"><span data-stu-id="78efd-167">The second parameter is the replacement string.</span></span> <span data-ttu-id="78efd-168">Třetí parametr, pokud existuje, určuje kód stavu.</span><span class="sxs-lookup"><span data-stu-id="78efd-168">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="78efd-169">Pokud nezadáte kód stavu, bude výchozí 302 (nalezeno), která označuje, že je prostředek dočasně přesunout nebo nahradit.</span><span class="sxs-lookup"><span data-stu-id="78efd-169">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="78efd-170">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="78efd-170">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="78efd-171">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="78efd-171">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

<span data-ttu-id="78efd-172">V prohlížeči pomocí nástrojů pro vývojáře, které jsou povolené, vytvořte žádost na ukázkové aplikace s cestou `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="78efd-172">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="78efd-173">Regex odpovídá cesta k požadavku na `redirect-rule/(.*)`, a je nahrazený cestu s `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="78efd-173">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="78efd-174">Přesměrování URL budou odeslána zpět do klienta se 302 stavový kód (Found).</span><span class="sxs-lookup"><span data-stu-id="78efd-174">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="78efd-175">V prohlížeči vytváří nový požadavek na adrese URL přesměrování, která se zobrazí v panelu Adresa prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="78efd-175">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="78efd-176">Vzhledem k tomu, že žádná pravidla v ukázkové aplikace odpovídají na adresu URL pro přesměrování, druhý požadavek obdrží odpověď 200 (OK) z aplikace a text odpovědi zobrazí adresa URL pro přesměrování.</span><span class="sxs-lookup"><span data-stu-id="78efd-176">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="78efd-177">Když je adresa URL přišla zvyšuje výkon serveru *přesměrováno*.</span><span class="sxs-lookup"><span data-stu-id="78efd-177">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="78efd-178">Buďte opatrní při vytváření pravidel přesměrování.</span><span class="sxs-lookup"><span data-stu-id="78efd-178">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="78efd-179">Přesměrování pravidla se vyhodnocují na každý požadavek pro aplikaci, včetně po přesměrování.</span><span class="sxs-lookup"><span data-stu-id="78efd-179">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="78efd-180">Ať už náhodně snadno vytvořit smyčku nekonečné přesměrování.</span><span class="sxs-lookup"><span data-stu-id="78efd-180">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="78efd-181">Původní žádost:`/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="78efd-181">Original Request: `/redirect-rule/1234/5678`</span></span>

![Okno prohlížeče pomocí nástrojů pro vývojáře, sledování požadavků a odpovědí](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="78efd-183">Je volána pro část výrazu uvést v uvozovkách *skupiny zachycení*.</span><span class="sxs-lookup"><span data-stu-id="78efd-183">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="78efd-184">Tečky (`.`) výrazu znamená *Porovná libovolný znak*.</span><span class="sxs-lookup"><span data-stu-id="78efd-184">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="78efd-185">Hvězdička (`*`) označuje *odpovídat předchozí znak vícekrát nebo*.</span><span class="sxs-lookup"><span data-stu-id="78efd-185">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="78efd-186">Proto segmenty posledních dvou cesty adresy URL, `1234/5678`, jsou zachyceny zachycení skupiny `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="78efd-186">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="78efd-187">Libovolná hodnota, které zadáte v adrese URL žádosti po `redirect-rule/` zachycenou této skupiny jediné zachycení.</span><span class="sxs-lookup"><span data-stu-id="78efd-187">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="78efd-188">V řetězci nahrazení zaznamenané skupiny jsou vloženy do řetězce s znak dolaru (`$`) následuje pořadové číslo zachytávání.</span><span class="sxs-lookup"><span data-stu-id="78efd-188">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="78efd-189">Získá první hodnota skupiny zachycení s `$1`, sekundu s `$2`, a budou pokračovat v pořadí pro zachycení skupiny ve vašem regulární výraz.</span><span class="sxs-lookup"><span data-stu-id="78efd-189">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="78efd-190">Je jenom jedna skupina zaznamenané regex pravidlo přesměrování v ukázkové aplikace, takže jenom jedné skupiny vloženého v řetězci nahrazení, který je `$1`.</span><span class="sxs-lookup"><span data-stu-id="78efd-190">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="78efd-191">Když se pravidlo se použije, stane se adresa URL `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="78efd-191">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

<a name="url-redirect-to-secure-endpoint"></a>
### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="78efd-192">Adresa URL přesměrování na zabezpečený koncový bod</span><span class="sxs-lookup"><span data-stu-id="78efd-192">URL redirect to a secure endpoint</span></span>
<span data-ttu-id="78efd-193">Použití `AddRedirectToHttps` pro přesměrování požadavků HTTP na stejné hostitele a cestu pomocí protokolu HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="78efd-193">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="78efd-194">Pokud je stavový kód není zadaný, middleware výchozí 302 (Found).</span><span class="sxs-lookup"><span data-stu-id="78efd-194">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="78efd-195">Pokud není port zadaný, middleware výchozí `null`, což znamená, že protokol změny `https://` a klient přistupuje k prostředku na portu 443.</span><span class="sxs-lookup"><span data-stu-id="78efd-195">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="78efd-196">Tento příklad ukazuje, jak nastavit stavový kód na 301 (trvale přesunut) a změnit na 5001.</span><span class="sxs-lookup"><span data-stu-id="78efd-196">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
<span data-ttu-id="78efd-197">Použití `AddRedirectToHttpsPermanent` přesměrovat nezabezpečené požadavky do stejné hostitele a cestu s zabezpečený protokol HTTPS (`https://` na portu 443).</span><span class="sxs-lookup"><span data-stu-id="78efd-197">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="78efd-198">Middleware nastaví stavový kód na 301 (trvale přesunut).</span><span class="sxs-lookup"><span data-stu-id="78efd-198">The middleware sets the status code to 301 (Moved Permanently).</span></span>

<span data-ttu-id="78efd-199">Ukázková aplikace je schopen který ukazuje, jak používat `AddRedirectToHttps` nebo `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="78efd-199">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="78efd-200">Add – metoda rozšíření pro `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="78efd-200">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="78efd-201">Proveďte požadavek nezabezpečené aplikace na všechny adresy URL.</span><span class="sxs-lookup"><span data-stu-id="78efd-201">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="78efd-202">Zavřete zabezpečení prohlížeče upozornění, že není důvěryhodný certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="78efd-202">Dismiss the browser security warning that the self-signed certificate is untrusted.</span></span>

<span data-ttu-id="78efd-203">Původní žádosti o pomocí `AddRedirectToHttps(301, 5001)`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="78efd-203">Original Request using `AddRedirectToHttps(301, 5001)`: `/secure`</span></span>

![Okno prohlížeče pomocí nástrojů pro vývojáře, sledování požadavků a odpovědí](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="78efd-205">Původní žádosti o pomocí `AddRedirectToHttpsPermanent`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="78efd-205">Original Request using `AddRedirectToHttpsPermanent`: `/secure`</span></span>

![Okno prohlížeče pomocí nástrojů pro vývojáře, sledování požadavků a odpovědí](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="78efd-207">Přepisování adres URL</span><span class="sxs-lookup"><span data-stu-id="78efd-207">URL rewrite</span></span>
<span data-ttu-id="78efd-208">Použití `AddRewrite` k vytvoření pravidel pro přepisování adres URL.</span><span class="sxs-lookup"><span data-stu-id="78efd-208">Use `AddRewrite` to create a rules for rewriting URLs.</span></span> <span data-ttu-id="78efd-209">První parametr obsahuje vaše regulární výraz k porovnání na příchozí cestě adresy URL.</span><span class="sxs-lookup"><span data-stu-id="78efd-209">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="78efd-210">Druhý parametr je náhradní řetězec.</span><span class="sxs-lookup"><span data-stu-id="78efd-210">The second parameter is the replacement string.</span></span> <span data-ttu-id="78efd-211">Třetí parametr `skipRemainingRules: {true|false}`, určuje pro middleware, zda se má vynechat přepisování další pravidla, pokud aktuální pravidlo se použije.</span><span class="sxs-lookup"><span data-stu-id="78efd-211">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="78efd-212">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="78efd-212">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="78efd-213">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="78efd-213">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

<span data-ttu-id="78efd-214">Původní žádost:`/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="78efd-214">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Okno prohlížeče pomocí nástrojů pro vývojáře, sledování požadavků a odpovědí](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="78efd-216">První věc, kterou jste si všimli regulární výraz je stříšky (`^`) na začátku výrazu.</span><span class="sxs-lookup"><span data-stu-id="78efd-216">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="78efd-217">To znamená, že odpovídající začne od začátku cesty URL.</span><span class="sxs-lookup"><span data-stu-id="78efd-217">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="78efd-218">V předchozím příkladu s tímto pravidlem přesměrování `redirect-rule/(.*)`, neexistuje žádný karátová na začátku regex; proto může předcházet znaky `redirect-rule/` v cestě pro úspěšné shodu.</span><span class="sxs-lookup"><span data-stu-id="78efd-218">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="78efd-219">Cesta</span><span class="sxs-lookup"><span data-stu-id="78efd-219">Path</span></span>                               | <span data-ttu-id="78efd-220">Shoda</span><span class="sxs-lookup"><span data-stu-id="78efd-220">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="78efd-221">Ano</span><span class="sxs-lookup"><span data-stu-id="78efd-221">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="78efd-222">Ano</span><span class="sxs-lookup"><span data-stu-id="78efd-222">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="78efd-223">Ano</span><span class="sxs-lookup"><span data-stu-id="78efd-223">Yes</span></span>   |

<span data-ttu-id="78efd-224">Přepište pravidlo `^rewrite-rule/(\d+)/(\d+)`, pouze odpovídá cesty, pokud jsou při spuštění systému `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="78efd-224">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="78efd-225">Všimněte si rozdíl mezi níže přepisování pravidla a výše uvedené pravidlo přesměrování s odpovídajícím.</span><span class="sxs-lookup"><span data-stu-id="78efd-225">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="78efd-226">Cesta</span><span class="sxs-lookup"><span data-stu-id="78efd-226">Path</span></span>                              | <span data-ttu-id="78efd-227">Shoda</span><span class="sxs-lookup"><span data-stu-id="78efd-227">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="78efd-228">Ano</span><span class="sxs-lookup"><span data-stu-id="78efd-228">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="78efd-229">Ne</span><span class="sxs-lookup"><span data-stu-id="78efd-229">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="78efd-230">Ne</span><span class="sxs-lookup"><span data-stu-id="78efd-230">No</span></span>    |

<span data-ttu-id="78efd-231">Následující `^rewrite-rule/` část výrazu, existují dvě skupiny zachycení `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="78efd-231">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="78efd-232">`\d` Označuje, že *odpovídat číslice (number)*.</span><span class="sxs-lookup"><span data-stu-id="78efd-232">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="78efd-233">Znaménko plus (`+`) znamená *odpovídat nejméně jeden z předchozí znak*.</span><span class="sxs-lookup"><span data-stu-id="78efd-233">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="78efd-234">Proto adresa URL musí obsahovat číslo následované lomítko následované jiné číslo.</span><span class="sxs-lookup"><span data-stu-id="78efd-234">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="78efd-235">Tyto zachycení skupiny jsou vloženy do rewritten adresu URL jako `$1` a `$2`.</span><span class="sxs-lookup"><span data-stu-id="78efd-235">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="78efd-236">Přepište pravidlo náhradní řetězec umístí zachycených skupin do řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="78efd-236">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="78efd-237">Požadovanou cestu `/rewrite-rule/1234/5678` je přepsaná získat prostředek na `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="78efd-237">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="78efd-238">Pokud řetězec dotazu je na původní žádost, ho se zachová, i když je přepsaná adresu URL.</span><span class="sxs-lookup"><span data-stu-id="78efd-238">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="78efd-239">Neexistuje žádná zpětná transformace k serveru a získat prostředek.</span><span class="sxs-lookup"><span data-stu-id="78efd-239">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="78efd-240">Pokud existuje prostředek má načtených a vrátí klientovi s 200 (OK) stavový kód.</span><span class="sxs-lookup"><span data-stu-id="78efd-240">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="78efd-241">Protože klient není přesměrován, adresu URL v adresním řádku prohlížeče se nemění.</span><span class="sxs-lookup"><span data-stu-id="78efd-241">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="78efd-242">Jak jde klienta, nikdy operaci přepsání adresy URL došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="78efd-242">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="78efd-243">Použití `skipRemainingRules: true` kdykoli je to možné, protože odpovídající pravidla je náročný proces a snižuje dobu odezvy aplikace.</span><span class="sxs-lookup"><span data-stu-id="78efd-243">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="78efd-244">Odpovědi na nejrychlejší aplikace:</span><span class="sxs-lookup"><span data-stu-id="78efd-244">For the fastest app response:</span></span>
> * <span data-ttu-id="78efd-245">Pořadí pravidel přepisování z nejčastěji odpovídající pravidla alespoň často odpovídající pravidlo.</span><span class="sxs-lookup"><span data-stu-id="78efd-245">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="78efd-246">Zpracování zbývající pravidla přeskočte, pokud je nalezena shoda a není nutná žádná další pravidla zpracování.</span><span class="sxs-lookup"><span data-stu-id="78efd-246">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="78efd-247">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="78efd-247">Apache mod_rewrite</span></span>
<span data-ttu-id="78efd-248">Použití Apache mod_rewrite pravidel s `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="78efd-248">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="78efd-249">Ujistěte se, že je soubor pravidla nasazen s aplikací.</span><span class="sxs-lookup"><span data-stu-id="78efd-249">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="78efd-250">Další informace a příklady pravidel mod_rewrite najdete v tématu [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="78efd-250">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="78efd-251">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="78efd-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="78efd-252">A `StreamReader` slouží k načtení pravidla ze *ApacheModRewrite.txt* soubor s pravidly.</span><span class="sxs-lookup"><span data-stu-id="78efd-252">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="78efd-253">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="78efd-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="78efd-254">První parametr má `IFileProvider`, který je k dispozici prostřednictvím [vkládání závislostí](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="78efd-254">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="78efd-255">`IHostingEnvironment` Je vložili zajistit `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="78efd-255">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="78efd-256">Druhý parametr je cesta k souboru pravidla, která je *ApacheModRewrite.txt* v ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="78efd-256">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

<span data-ttu-id="78efd-257">Ukázková aplikace přesměruje požadavky z `/apache-mod-rules-redirect/(.\*)` k `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="78efd-257">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="78efd-258">Stavový kód odpovědi je 302 (Found).</span><span class="sxs-lookup"><span data-stu-id="78efd-258">The response status code is 302 (Found).</span></span>

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

<span data-ttu-id="78efd-259">Původní žádost:`/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="78efd-259">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Okno prohlížeče pomocí nástrojů pro vývojáře, sledování požadavků a odpovědí](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="78efd-261">Podporované serverových proměnných</span><span class="sxs-lookup"><span data-stu-id="78efd-261">Supported server variables</span></span>
<span data-ttu-id="78efd-262">Middleware podporuje následující proměnné serveru Apache mod_rewrite:</span><span class="sxs-lookup"><span data-stu-id="78efd-262">The middleware supports the following Apache mod_rewrite server variables:</span></span>
* <span data-ttu-id="78efd-263">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="78efd-263">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="78efd-264">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="78efd-264">HTTP_ACCEPT</span></span>
* <span data-ttu-id="78efd-265">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="78efd-265">HTTP_CONNECTION</span></span>
* <span data-ttu-id="78efd-266">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="78efd-266">HTTP_COOKIE</span></span>
* <span data-ttu-id="78efd-267">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="78efd-267">HTTP_FORWARDED</span></span>
* <span data-ttu-id="78efd-268">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="78efd-268">HTTP_HOST</span></span>
* <span data-ttu-id="78efd-269">HTTP_REFERER, ABYSTE</span><span class="sxs-lookup"><span data-stu-id="78efd-269">HTTP_REFERER</span></span>
* <span data-ttu-id="78efd-270">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="78efd-270">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="78efd-271">PROTOKOL HTTPS</span><span class="sxs-lookup"><span data-stu-id="78efd-271">HTTPS</span></span>
* <span data-ttu-id="78efd-272">IPV6</span><span class="sxs-lookup"><span data-stu-id="78efd-272">IPV6</span></span>
* <span data-ttu-id="78efd-273">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="78efd-273">QUERY_STRING</span></span>
* <span data-ttu-id="78efd-274">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="78efd-274">REMOTE_ADDR</span></span>
* <span data-ttu-id="78efd-275">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="78efd-275">REMOTE_PORT</span></span>
* <span data-ttu-id="78efd-276">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="78efd-276">REQUEST_FILENAME</span></span>
* <span data-ttu-id="78efd-277">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="78efd-277">REQUEST_METHOD</span></span>
* <span data-ttu-id="78efd-278">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="78efd-278">REQUEST_SCHEME</span></span>
* <span data-ttu-id="78efd-279">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="78efd-279">REQUEST_URI</span></span>
* <span data-ttu-id="78efd-280">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="78efd-280">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="78efd-281">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="78efd-281">SERVER_ADDR</span></span>
* <span data-ttu-id="78efd-282">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="78efd-282">SERVER_PORT</span></span>
* <span data-ttu-id="78efd-283">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="78efd-283">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="78efd-284">ČAS</span><span class="sxs-lookup"><span data-stu-id="78efd-284">TIME</span></span>
* <span data-ttu-id="78efd-285">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="78efd-285">TIME_DAY</span></span>
* <span data-ttu-id="78efd-286">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="78efd-286">TIME_HOUR</span></span>
* <span data-ttu-id="78efd-287">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="78efd-287">TIME_MIN</span></span>
* <span data-ttu-id="78efd-288">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="78efd-288">TIME_MON</span></span>
* <span data-ttu-id="78efd-289">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="78efd-289">TIME_SEC</span></span>
* <span data-ttu-id="78efd-290">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="78efd-290">TIME_WDAY</span></span>
* <span data-ttu-id="78efd-291">MODULU KPI</span><span class="sxs-lookup"><span data-stu-id="78efd-291">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="78efd-292">Modul přepisování adres URL služby IIS pravidla</span><span class="sxs-lookup"><span data-stu-id="78efd-292">IIS URL Rewrite Module rules</span></span>
<span data-ttu-id="78efd-293">Chcete-li použít pravidla, která se týkají modul přepisování adres URL služby IIS, použijte `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="78efd-293">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="78efd-294">Ujistěte se, že je soubor pravidla nasazen s aplikací.</span><span class="sxs-lookup"><span data-stu-id="78efd-294">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="78efd-295">Nemáte přímé middlewaru, který má být použit vaše *web.config* souborů při spuštění na Windows serveru IIS.</span><span class="sxs-lookup"><span data-stu-id="78efd-295">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="78efd-296">Se službou IIS, by měly být tato pravidla uložené mimo vaší *web.config* aby nedocházelo ke konfliktům s modulem přepisu služby IIS.</span><span class="sxs-lookup"><span data-stu-id="78efd-296">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="78efd-297">Další informace a příklady pravidel modul přepisování adres URL služby IIS najdete v tématu [pomocí přepisování adres Url modulu 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) a [URL přepisování konfigurace odkazu na modul](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="78efd-297">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="78efd-298">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="78efd-298">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="78efd-299">A `StreamReader` slouží k načtení pravidla ze *IISUrlRewrite.xml* soubor s pravidly.</span><span class="sxs-lookup"><span data-stu-id="78efd-299">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="78efd-300">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="78efd-300">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="78efd-301">První parametr má `IFileProvider`, zatímco druhý parametr je cesta k souboru XML pravidla, která je *IISUrlRewrite.xml* v ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="78efd-301">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

<span data-ttu-id="78efd-302">Ukázková aplikace přepíše požadavky od `/iis-rules-rewrite/(.*)` k `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="78efd-302">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="78efd-303">Odpověď je odeslána na klienta se 200 stavový kód (OK).</span><span class="sxs-lookup"><span data-stu-id="78efd-303">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

<span data-ttu-id="78efd-304">Původní žádost:`/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="78efd-304">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Okno prohlížeče pomocí nástrojů pro vývojáře, sledování požadavků a odpovědí](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="78efd-306">Pokud máte aktivní přepisování modulu IIS s nakonfigurovaná pravidla úrovni serveru, které by ovlivnit aplikace nežádoucího způsoby, můžete zakázat modul přepisování služby IIS pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="78efd-306">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="78efd-307">Další informace najdete v tématu [moduly IIS zakázání](xref:hosting/iis-modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="78efd-307">For more information, see [Disabling IIS modules](xref:hosting/iis-modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="78efd-308">Nepodporované funkce</span><span class="sxs-lookup"><span data-stu-id="78efd-308">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="78efd-309">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="78efd-309">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="78efd-310">Middleware vydané s ASP.NET Core 2.x nepodporuje následující funkce modul přepisování adres URL služby IIS:</span><span class="sxs-lookup"><span data-stu-id="78efd-310">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="78efd-311">Odchozí pravidla</span><span class="sxs-lookup"><span data-stu-id="78efd-311">Outbound Rules</span></span>
* <span data-ttu-id="78efd-312">Vlastní serverové proměnné</span><span class="sxs-lookup"><span data-stu-id="78efd-312">Custom Server Variables</span></span>
* <span data-ttu-id="78efd-313">Zástupné znaky</span><span class="sxs-lookup"><span data-stu-id="78efd-313">Wildcards</span></span>
* <span data-ttu-id="78efd-314">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="78efd-314">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="78efd-315">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="78efd-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="78efd-316">Middleware vydané s ASP.NET Core 1.x nepodporuje následující funkce modul přepisování adres URL služby IIS:</span><span class="sxs-lookup"><span data-stu-id="78efd-316">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="78efd-317">Globální pravidla</span><span class="sxs-lookup"><span data-stu-id="78efd-317">Global Rules</span></span>
* <span data-ttu-id="78efd-318">Odchozí pravidla</span><span class="sxs-lookup"><span data-stu-id="78efd-318">Outbound Rules</span></span>
* <span data-ttu-id="78efd-319">Přepište mapy</span><span class="sxs-lookup"><span data-stu-id="78efd-319">Rewrite Maps</span></span>
* <span data-ttu-id="78efd-320">CustomResponse akce</span><span class="sxs-lookup"><span data-stu-id="78efd-320">CustomResponse Action</span></span>
* <span data-ttu-id="78efd-321">Vlastní serverové proměnné</span><span class="sxs-lookup"><span data-stu-id="78efd-321">Custom Server Variables</span></span>
* <span data-ttu-id="78efd-322">Zástupné znaky</span><span class="sxs-lookup"><span data-stu-id="78efd-322">Wildcards</span></span>
* <span data-ttu-id="78efd-323">Akce: CustomResponse</span><span class="sxs-lookup"><span data-stu-id="78efd-323">Action:CustomResponse</span></span>
* <span data-ttu-id="78efd-324">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="78efd-324">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="78efd-325">Podporované serverových proměnných</span><span class="sxs-lookup"><span data-stu-id="78efd-325">Supported server variables</span></span>
<span data-ttu-id="78efd-326">Middleware podporuje následující proměnné serveru modul přepisování adres URL služby IIS:</span><span class="sxs-lookup"><span data-stu-id="78efd-326">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>
* <span data-ttu-id="78efd-327">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="78efd-327">CONTENT_LENGTH</span></span>
* <span data-ttu-id="78efd-328">TYP_OBSAHU</span><span class="sxs-lookup"><span data-stu-id="78efd-328">CONTENT_TYPE</span></span>
* <span data-ttu-id="78efd-329">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="78efd-329">HTTP_ACCEPT</span></span>
* <span data-ttu-id="78efd-330">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="78efd-330">HTTP_CONNECTION</span></span>
* <span data-ttu-id="78efd-331">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="78efd-331">HTTP_COOKIE</span></span>
* <span data-ttu-id="78efd-332">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="78efd-332">HTTP_HOST</span></span>
* <span data-ttu-id="78efd-333">HTTP_REFERER, ABYSTE</span><span class="sxs-lookup"><span data-stu-id="78efd-333">HTTP_REFERER</span></span>
* <span data-ttu-id="78efd-334">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="78efd-334">HTTP_URL</span></span>
* <span data-ttu-id="78efd-335">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="78efd-335">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="78efd-336">PROTOKOL HTTPS</span><span class="sxs-lookup"><span data-stu-id="78efd-336">HTTPS</span></span>
* <span data-ttu-id="78efd-337">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="78efd-337">LOCAL_ADDR</span></span>
* <span data-ttu-id="78efd-338">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="78efd-338">QUERY_STRING</span></span>
* <span data-ttu-id="78efd-339">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="78efd-339">REMOTE_ADDR</span></span>
* <span data-ttu-id="78efd-340">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="78efd-340">REMOTE_PORT</span></span>
* <span data-ttu-id="78efd-341">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="78efd-341">REQUEST_FILENAME</span></span>
* <span data-ttu-id="78efd-342">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="78efd-342">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="78efd-343">Můžete také získat `IFileProvider` prostřednictvím `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="78efd-343">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="78efd-344">Tento přístup může poskytují větší flexibilitu při umístění vaší přepisování pravidla soubory.</span><span class="sxs-lookup"><span data-stu-id="78efd-344">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="78efd-345">Ujistěte se, že vaše soubory přepisování pravidel jsou nasazeny na server v cestu, kterou zadáte.</span><span class="sxs-lookup"><span data-stu-id="78efd-345">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="78efd-346">Pravidla na základě – metoda</span><span class="sxs-lookup"><span data-stu-id="78efd-346">Method-based rule</span></span>
<span data-ttu-id="78efd-347">Použití `Add(Action<RewriteContext> applyRule)` implementace vlastní logiky na metodu.</span><span class="sxs-lookup"><span data-stu-id="78efd-347">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="78efd-348">`RewriteContext` Zpřístupní `HttpContext` pro použití ve své metodě.</span><span class="sxs-lookup"><span data-stu-id="78efd-348">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="78efd-349">`context.Result` Určuje, jak další kanálu zpracování se zpracovává.</span><span class="sxs-lookup"><span data-stu-id="78efd-349">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="78efd-350">kontext. Výsledek</span><span class="sxs-lookup"><span data-stu-id="78efd-350">context.Result</span></span>                       | <span data-ttu-id="78efd-351">Akce</span><span class="sxs-lookup"><span data-stu-id="78efd-351">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="78efd-352">`RuleResult.ContinueRules`(výchozí)</span><span class="sxs-lookup"><span data-stu-id="78efd-352">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="78efd-353">Pokračovat v použití pravidel</span><span class="sxs-lookup"><span data-stu-id="78efd-353">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="78efd-354">Zastavte aplikaci pravidel a odeslání odpovědi</span><span class="sxs-lookup"><span data-stu-id="78efd-354">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="78efd-355">Zastavte aplikaci pravidel a kontext poslat další middleware</span><span class="sxs-lookup"><span data-stu-id="78efd-355">Stop applying rules and send the context to the next middleware</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="78efd-356">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="78efd-356">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="78efd-357">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="78efd-357">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

<span data-ttu-id="78efd-358">Ukázková aplikace ukazuje metodu, který přesměruje požadavky pro cesty, která končit *.xml*.</span><span class="sxs-lookup"><span data-stu-id="78efd-358">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="78efd-359">Pokud zadáte požadavek `/file.xml`, je přesměrován na `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="78efd-359">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="78efd-360">Kód stavu je nastavena na 301 (trvale přesunut).</span><span class="sxs-lookup"><span data-stu-id="78efd-360">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="78efd-361">Pro přesměrování je potřeba explicitně nastavit stavový kód odpovědi; jinak se vrátí stavový kód 200 (OK) a přesměrování nedojde v klientovi.</span><span class="sxs-lookup"><span data-stu-id="78efd-361">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="78efd-362">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="78efd-362">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="78efd-363">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="78efd-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

<span data-ttu-id="78efd-364">Původní žádost:`/file.xml`</span><span class="sxs-lookup"><span data-stu-id="78efd-364">Original Request: `/file.xml`</span></span>

![Okno prohlížeče s sledování požadavků a odpovědí pro file.xml nástroje pro vývojáře](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="78efd-366">Na základě IRule pravidlo</span><span class="sxs-lookup"><span data-stu-id="78efd-366">IRule-based rule</span></span>
<span data-ttu-id="78efd-367">Použití `Add(IRule)` implementace vlastní logiky na třídu odvozenou z `IRule`.</span><span class="sxs-lookup"><span data-stu-id="78efd-367">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="78efd-368">Pomocí `IRule` nabízí větší flexibilitu v porovnání s pomocí přístupu na základě metod pravidlo.</span><span class="sxs-lookup"><span data-stu-id="78efd-368">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="78efd-369">Odvozené třídy mohou zahrnovat konstruktor, kde můžete předat parametry pro `ApplyRule` metoda.</span><span class="sxs-lookup"><span data-stu-id="78efd-369">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="78efd-370">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="78efd-370">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="78efd-371">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="78efd-371">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

<span data-ttu-id="78efd-372">Hodnoty parametrů v ukázková aplikace pro `extension` a `newPath` se kontroluje, že splňují několik podmínek.</span><span class="sxs-lookup"><span data-stu-id="78efd-372">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="78efd-373">`extension` Musí obsahovat hodnotu, a hodnota musí být *.png*, *.jpg*, nebo *.gif*.</span><span class="sxs-lookup"><span data-stu-id="78efd-373">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="78efd-374">Pokud `newPath` není platný, `ArgumentException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="78efd-374">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="78efd-375">Pokud zadáte požadavek *image.png*, je přesměrován na `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="78efd-375">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="78efd-376">Pokud zadáte požadavek *image.jpg*, je přesměrován na `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="78efd-376">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="78efd-377">Kód stavu je nastavena na 301 (trvale přesunuto) a `context.Result` je nastavena na Zastavit zpracování pravidel a odeslání odpovědi.</span><span class="sxs-lookup"><span data-stu-id="78efd-377">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="78efd-378">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="78efd-378">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="78efd-379">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="78efd-379">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

<span data-ttu-id="78efd-380">Původní žádost:`/image.png`</span><span class="sxs-lookup"><span data-stu-id="78efd-380">Original Request: `/image.png`</span></span>

![Okno prohlížeče s sledování požadavků a odpovědí pro image.png nástroje pro vývojáře](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="78efd-382">Původní žádost:`/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="78efd-382">Original Request: `/image.jpg`</span></span>

![Okno prohlížeče s sledování požadavků a odpovědí pro image.jpg nástroje pro vývojáře](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="78efd-384">Příklady Regex</span><span class="sxs-lookup"><span data-stu-id="78efd-384">Regex examples</span></span>

| <span data-ttu-id="78efd-385">Cílem</span><span class="sxs-lookup"><span data-stu-id="78efd-385">Goal</span></span> | <span data-ttu-id="78efd-386">Řetězec Regex &</span><span class="sxs-lookup"><span data-stu-id="78efd-386">Regex String &</span></span><br><span data-ttu-id="78efd-387">Příklad shody</span><span class="sxs-lookup"><span data-stu-id="78efd-387">Match Example</span></span> | <span data-ttu-id="78efd-388">Náhradní řetězec &</span><span class="sxs-lookup"><span data-stu-id="78efd-388">Replacement String &</span></span><br><span data-ttu-id="78efd-389">Příklad výstupu</span><span class="sxs-lookup"><span data-stu-id="78efd-389">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="78efd-390">Přepište cestu do řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="78efd-390">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="78efd-391">Pruhu koncové lomítko.</span><span class="sxs-lookup"><span data-stu-id="78efd-391">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="78efd-392">Vynutit koncové lomítko</span><span class="sxs-lookup"><span data-stu-id="78efd-392">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="78efd-393">Vyhněte se přepisování konkrétní požadavky</span><span class="sxs-lookup"><span data-stu-id="78efd-393">Avoid rewriting specific requests</span></span> | `(.*[^(\.axd)])$`<br><span data-ttu-id="78efd-394">Ano:`/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="78efd-394">Yes: `/resource.htm`</span></span><br><span data-ttu-id="78efd-395">Ne:`/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="78efd-395">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="78efd-396">Změna uspořádání segmenty adres URL</span><span class="sxs-lookup"><span data-stu-id="78efd-396">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="78efd-397">Nahraďte segment adresy URL</span><span class="sxs-lookup"><span data-stu-id="78efd-397">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="78efd-398">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="78efd-398">Additional resources</span></span>
* [<span data-ttu-id="78efd-399">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="78efd-399">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="78efd-400">Middleware</span><span class="sxs-lookup"><span data-stu-id="78efd-400">Middleware</span></span>](middleware.md)
* [<span data-ttu-id="78efd-401">Regulární výrazy v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="78efd-401">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="78efd-402">Jazyk regulárních výrazů – Stručná referenční příručka</span><span class="sxs-lookup"><span data-stu-id="78efd-402">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="78efd-403">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="78efd-403">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="78efd-404">Pomocí modulu přepisování Url 2.0 (pro službu IIS)</span><span class="sxs-lookup"><span data-stu-id="78efd-404">Using Url Rewrite Module 2.0 (for IIS)</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="78efd-405">Odkaz na modul konfigurace adresy URL přepsání</span><span class="sxs-lookup"><span data-stu-id="78efd-405">URL Rewrite Module Configuration Reference</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="78efd-406">Fórum modul přepisování adresa URL služby IIS</span><span class="sxs-lookup"><span data-stu-id="78efd-406">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="78efd-407">Ponechat jednoduchou strukturu adresy URL</span><span class="sxs-lookup"><span data-stu-id="78efd-407">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="78efd-408">10 přepisování adres URL tipy a triky</span><span class="sxs-lookup"><span data-stu-id="78efd-408">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="78efd-409">Lomítko nebo lomítko</span><span class="sxs-lookup"><span data-stu-id="78efd-409">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
