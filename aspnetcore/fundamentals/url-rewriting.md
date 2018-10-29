---
title: Middleware v ASP.NET Core přepisování adres URL
author: guardrex
description: Další informace o adrese URL přepsání a přesměrování s Middleware pro přepis adres URL v aplikacích ASP.NET Core.
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: 5a1891c838436467fb49ff6288587fab08201179
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207183"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="48110-103">Middleware v ASP.NET Core přepisování adres URL</span><span class="sxs-lookup"><span data-stu-id="48110-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="48110-104">Podle [Luke Latham](https://github.com/guardrex) a [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="48110-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="48110-105">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([stažení](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="48110-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="48110-106">Přepisování adres URL je úprava žádosti o adresy URL v závislosti na jeden nebo více předdefinovaných pravidel.</span><span class="sxs-lookup"><span data-stu-id="48110-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="48110-107">Přepisování adres URL vytvoří abstrakci mezi umístění prostředků a jejich adresy tak, aby umístění a adresy nejsou připojeny úzce.</span><span class="sxs-lookup"><span data-stu-id="48110-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="48110-108">Existuje několik scénářů, kdy je vhodné přepisování adres URL:</span><span class="sxs-lookup"><span data-stu-id="48110-108">There are several scenarios where URL rewriting is valuable:</span></span>

* <span data-ttu-id="48110-109">Přesunutí nebo nahrazení prostředky serveru dočasně nebo trvale při zachování stabilní lokátory pro tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="48110-109">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources.</span></span>
* <span data-ttu-id="48110-110">Zpracování v různých aplikací nebo víc oblastech jednu aplikaci rozdělení požadavků.</span><span class="sxs-lookup"><span data-stu-id="48110-110">Splitting request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="48110-111">Odebrání, přidání nebo změna uspořádání segmenty adres URL na příchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="48110-111">Removing, adding, or reorganizing URL segments on incoming requests.</span></span>
* <span data-ttu-id="48110-112">Optimalizace veřejné adresy URL pro hledání modulu optimalizace (SEO).</span><span class="sxs-lookup"><span data-stu-id="48110-112">Optimizing public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="48110-113">Umožňující použití popisný veřejné adresy URL, který pomůže uživatelům předpovědět, obsah, který se najdou pomocí následujícího odkazu.</span><span class="sxs-lookup"><span data-stu-id="48110-113">Permitting the use of friendly public URLs to help people predict the content they will find by following a link.</span></span>
* <span data-ttu-id="48110-114">Přesměrování nezabezpečené požadavky na zabezpečení koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="48110-114">Redirecting insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="48110-115">Brání hotlinking bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="48110-115">Preventing image hotlinking.</span></span>

<span data-ttu-id="48110-116">Můžete definovat pravidla pro změny adresy URL několika způsoby, včetně regulárního výrazu, Apache mod_rewrite modul pravidel, pravidel modul služby IIS a pomocí logiky vlastní pravidlo.</span><span class="sxs-lookup"><span data-stu-id="48110-116">You can define rules for changing the URL in several ways, including Regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="48110-117">Toto téma představuje přepisování adres URL s pokyny, jak používat Middleware pro přepis adres URL v aplikacích ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="48110-117">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="48110-118">Přepisování adres URL, může snížit výkon aplikace.</span><span class="sxs-lookup"><span data-stu-id="48110-118">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="48110-119">Tam, kde je to proveditelné, měli omezit počet a složitost pravidel.</span><span class="sxs-lookup"><span data-stu-id="48110-119">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="48110-120">Adresa URL pro přesměrování a URL revize</span><span class="sxs-lookup"><span data-stu-id="48110-120">URL redirect and URL rewrite</span></span>

<span data-ttu-id="48110-121">Rozdíl mezi formulace *adresy URL přesměrování* a *přepsání adresy URL* se může zdát drobným na první, ale má důležité důsledky pro poskytování prostředků pro klienty.</span><span class="sxs-lookup"><span data-stu-id="48110-121">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="48110-122">Middleware pro přepis adres URL ASP.NET Core je schopen splnit potřebu obojí.</span><span class="sxs-lookup"><span data-stu-id="48110-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="48110-123">A *adresy URL přesměrování* je operace na straně klienta, kde klientovi je odeslán pokyn k přístupu k prostředku na jinou adresu.</span><span class="sxs-lookup"><span data-stu-id="48110-123">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="48110-124">To vyžaduje zpátečního převodu na server.</span><span class="sxs-lookup"><span data-stu-id="48110-124">This requires a round-trip to the server.</span></span> <span data-ttu-id="48110-125">Adresa URL přesměrování, který je vrácen do klienta se zobrazí v adresním řádku prohlížeče, pokud klient odešle novou žádost o prostředku.</span><span class="sxs-lookup"><span data-stu-id="48110-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> 

<span data-ttu-id="48110-126">Pokud `/resource` je *přesměrováno* k `/different-resource`, požadavky klientů `/resource`.</span><span class="sxs-lookup"><span data-stu-id="48110-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`.</span></span> <span data-ttu-id="48110-127">Tento server odpoví, že klient by měl získat prostředek na `/different-resource` s kód stavu označující, že přesměrování je dočasný nebo trvalý.</span><span class="sxs-lookup"><span data-stu-id="48110-127">The server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="48110-128">Klient spustí nového požadavku na prostředek adresy URL pro přesměrování.</span><span class="sxs-lookup"><span data-stu-id="48110-128">The client executes a new request for the resource at the redirect URL.</span></span>

![Koncový bod služby WebAPI dočasně změnila z verze 1 (v1) verze 2 (v2) na serveru.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="48110-134">Při přesměrování požadavků na jinou adresu URL, určujete, jestli je přesměrování trvalé nebo dočasné.</span><span class="sxs-lookup"><span data-stu-id="48110-134">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="48110-135">301 (trvale přesunuto) stavový kód se používá ve kterém prostředek má novou, trvalé adresy URL a chcete, aby dáte pokyn, aby klient, všechny budoucí žádosti o prostředku by měl použít novou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="48110-135">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="48110-136">*Klient může odpověď do mezipaměti, když je obdržena 301 stavový kód.*</span><span class="sxs-lookup"><span data-stu-id="48110-136">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="48110-137">302 stavový kód (Found) se používá, kde přesměrování je dočasná nebo obecně subjektu změnit tak, aby klient by neměly ukládat a opakovaně používat adresy URL pro přesměrování v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="48110-137">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="48110-138">Další informace najdete v tématu [dokumentu RFC 2616: definice stavových kódů](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="48110-138">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="48110-139">A *přepsání adresy URL* je operace na straně serveru k poskytování prostředků z adresy jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="48110-139">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="48110-140">Přepisování adres URL nevyžaduje zpátečního převodu na server.</span><span class="sxs-lookup"><span data-stu-id="48110-140">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="48110-141">Přepsaný adresa URL není vrácen do klienta a nezobrazí se v adresním řádku prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="48110-141">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="48110-142">Když `/resource` je *přepsán* k `/different-resource`, požadavky klientů `/resource`a server *interně* načte prostředek na `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="48110-142">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="48110-143">I když se klient může být schopný načíst prostředek na přepsaný adresu URL, nebude klient informován, že prostředky existují na přepsaný adrese URL, když vytvoří jeho žádost a obdrží odpověď.</span><span class="sxs-lookup"><span data-stu-id="48110-143">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Koncový bod služby WebAPI se změnil z verze 1 (v1) verze 2 (v2) na serveru.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="48110-148">Ukázková aplikace přepis adres URL</span><span class="sxs-lookup"><span data-stu-id="48110-148">URL rewriting sample app</span></span>

<span data-ttu-id="48110-149">Můžete prozkoumat funkce Middleware přepisování adres URL se [ukázkovou aplikaci přepis adres URL](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span><span class="sxs-lookup"><span data-stu-id="48110-149">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span></span> <span data-ttu-id="48110-150">Aplikace použije revize a přesměrování pravidla a zobrazuje přepsaný nebo přesměrované adresu URL.</span><span class="sxs-lookup"><span data-stu-id="48110-150">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="48110-151">Kdy použít Middleware pro přepis adres URL</span><span class="sxs-lookup"><span data-stu-id="48110-151">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="48110-152">Použijte Middleware pro přepis adres URL, pokud nemůžete použít [modul přepisování adres URL](https://www.iis.net/downloads/microsoft/url-rewrite) službou IIS v systému Windows Server, [Apache mod_rewrite modulu](https://httpd.apache.org/docs/2.4/rewrite/) na serveru Apache [přepisování adres URL na serveru Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), nebo je vaše aplikace hostovaná na [serveru HTTP.sys](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="48110-152">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="48110-153">Hlavní důvody k používání přepis adres URL serverovými technologiemi v IIS, Apache nebo Nginx se, že middleware nepodporují úplné funkce aplikace tyto moduly výkonu middleware pravděpodobně nebude odpovídat moduly.</span><span class="sxs-lookup"><span data-stu-id="48110-153">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="48110-154">Existují však některé funkce serveru moduly, která nepracují s projekty ASP.NET Core, jako `IsFile` a `IsDirectory` omezení modulu IIS Rewrite.</span><span class="sxs-lookup"><span data-stu-id="48110-154">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="48110-155">V těchto scénářích platí místo toho použijte middleware.</span><span class="sxs-lookup"><span data-stu-id="48110-155">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="48110-156">Balíček</span><span class="sxs-lookup"><span data-stu-id="48110-156">Package</span></span>

<span data-ttu-id="48110-157">Aby byly middleware ve vašem projektu, přidejte odkaz na [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="48110-157">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="48110-158">Tato funkce je k dispozici pro aplikace, které cílí ASP.NET Core 1.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="48110-158">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="48110-159">Rozšíření a možnosti</span><span class="sxs-lookup"><span data-stu-id="48110-159">Extension and options</span></span>

<span data-ttu-id="48110-160">Vytvoření vašeho přepsání adresy URL a přesměrovat pravidla tak, že vytvoříte instanci [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) třída s atributem rozšiřující metody pro každý z vašich pravidel.</span><span class="sxs-lookup"><span data-stu-id="48110-160">Establish your URL rewrite and redirect rules by creating an instance of the [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) class with extension methods for each of your rules.</span></span> <span data-ttu-id="48110-161">Zřetězit více pravidel v pořadí, které chcete je zpracována.</span><span class="sxs-lookup"><span data-stu-id="48110-161">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="48110-162">`RewriteOptions` Jsou předány do Middleware pro přepis adres URL, protože se přidá do kanálu požadavku s `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="48110-162">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="redirect-non-www-to-www"></a><span data-ttu-id="48110-163">Přesměrovat na www webové služby.</span><span class="sxs-lookup"><span data-stu-id="48110-163">Redirect non-www to www</span></span>

<span data-ttu-id="48110-164">Povolení aplikace pro přesměrování jinou hodnotu než tři možnosti`www` vyžádá, aby `www`:</span><span class="sxs-lookup"><span data-stu-id="48110-164">Three options permit the app to redirect non-`www` requests to `www`:</span></span>

* <span data-ttu-id="48110-165">[AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; požadavek na trvalé přesměrování `www` subdomény, pokud je požadavek jinou hodnotu než`www`.</span><span class="sxs-lookup"><span data-stu-id="48110-165">[AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; Permanently redirect the request to the `www` subdomain if the request is non-`www`.</span></span> <span data-ttu-id="48110-166">Přesměruje se [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect) stavový kód.</span><span class="sxs-lookup"><span data-stu-id="48110-166">Redirects with a [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect) status code.</span></span>
* <span data-ttu-id="48110-167">[AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; přesměrovat žádosti `www` subdomény, pokud je příchozí požadavek jinou hodnotu než`www`.</span><span class="sxs-lookup"><span data-stu-id="48110-167">[AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="48110-168">Přesměruje se [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) stavový kód.</span><span class="sxs-lookup"><span data-stu-id="48110-168">Redirects with a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) status code.</span></span>
* <span data-ttu-id="48110-169">[AddRedirectToWww (RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; přesměrovat žádosti `www` subdomény, pokud je příchozí požadavek jinou hodnotu než`www`.</span><span class="sxs-lookup"><span data-stu-id="48110-169">[AddRedirectToWww(RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="48110-170">Umožňuje poskytovat pro odpověď se stavovým kódem.</span><span class="sxs-lookup"><span data-stu-id="48110-170">Allows you to provide the status code for the response.</span></span> <span data-ttu-id="48110-171">Použijte pole [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) třídu pro přiřazení k `AddRedirectToWww`.</span><span class="sxs-lookup"><span data-stu-id="48110-171">Use the fields of the [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) class for assignments to `AddRedirectToWww`.</span></span>

::: moniker-end

### <a name="url-redirect"></a><span data-ttu-id="48110-172">Adresa URL pro přesměrování</span><span class="sxs-lookup"><span data-stu-id="48110-172">URL redirect</span></span>

<span data-ttu-id="48110-173">Použití `AddRedirect` pro přesměrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="48110-173">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="48110-174">První parametr obsahuje vaše regulárního výrazu pro porovnávání v cestě adresy URL příchozích.</span><span class="sxs-lookup"><span data-stu-id="48110-174">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="48110-175">Druhý parametr je řetězci pro nahrazení.</span><span class="sxs-lookup"><span data-stu-id="48110-175">The second parameter is the replacement string.</span></span> <span data-ttu-id="48110-176">Třetí parametr, pokud jsou k dispozici, určuje kód stavu.</span><span class="sxs-lookup"><span data-stu-id="48110-176">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="48110-177">Pokud nezadáte kód stavu, použije se výchozí 302 (Found), což znamená, že prostředek je dočasně nepřesunul ani nahradit.</span><span class="sxs-lookup"><span data-stu-id="48110-177">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="48110-178">V prohlížeči pomocí nástrojů pro vývojáře, vytvořte žádost na ukázkovou aplikaci s cestou `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="48110-178">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="48110-179">Odpovídá regulárnímu cestu požadavku na `redirect-rule/(.*)`, a cesta se nahradí `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="48110-179">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="48110-180">Přesměrování URL budou odeslána zpět do klienta se 302 stavovým kódem (Found).</span><span class="sxs-lookup"><span data-stu-id="48110-180">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="48110-181">Prohlížeč odešle nový požadavek na adrese URL přesměrování, které se zobrazí v adresním řádku prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="48110-181">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="48110-182">Vzhledem k tomu, že žádná pravidla v ukázkové aplikaci odpovídat na adresu URL pro přesměrování, druhou žádost přijme odpověď 200 (OK) z aplikace a tělo odpovědi zobrazí adresu URL přesměrování.</span><span class="sxs-lookup"><span data-stu-id="48110-182">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="48110-183">Zpětná je provedeno na server, když je adresa URL *přesměrováno*.</span><span class="sxs-lookup"><span data-stu-id="48110-183">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="48110-184">Buďte opatrní při vytváření pravidla pro přesměrování.</span><span class="sxs-lookup"><span data-stu-id="48110-184">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="48110-185">Pro každý požadavek na aplikaci, včetně po přesměrování se vyhodnocují pravidla pro přesměrování.</span><span class="sxs-lookup"><span data-stu-id="48110-185">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="48110-186">Je snadné neúmyslně vytvořit smyčku nekonečné přesměrování.</span><span class="sxs-lookup"><span data-stu-id="48110-186">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="48110-187">Původní žádosti: `/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="48110-187">Original Request: `/redirect-rule/1234/5678`</span></span>

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="48110-189">Část výraz v závorkách se nazývá *skupina zachycení*.</span><span class="sxs-lookup"><span data-stu-id="48110-189">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="48110-190">Tečka (`.`) výrazu znamená, že *odpovídá jakémukoli znaku*.</span><span class="sxs-lookup"><span data-stu-id="48110-190">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="48110-191">Hvězdička (`*`) označuje *odpovídá předchozímu znaku nula nebo vícekrát*.</span><span class="sxs-lookup"><span data-stu-id="48110-191">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="48110-192">Proto segmenty poslední dvě cesty adresy URL, `1234/5678`, jsou zachyceny na základě skupina zachycení `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="48110-192">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="48110-193">Libovolná hodnota je zadat v adrese URL požadavku po `redirect-rule/` zachycena této skupiny jediné zachycení.</span><span class="sxs-lookup"><span data-stu-id="48110-193">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="48110-194">V řetězci pro nahrazení zachycené skupiny jsou vloženy do řetězce bez znak dolaru (`$`) následované pořadové číslo pro zachytávání.</span><span class="sxs-lookup"><span data-stu-id="48110-194">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="48110-195">Hodnota první skupiny zachycení se získá pomocí `$1`, druhý s `$2`, a budou pokračovat v práci v pořadí pro zachycení skupiny ve vaší regulární výraz.</span><span class="sxs-lookup"><span data-stu-id="48110-195">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="48110-196">Je pouze jeden zachycené skupině regex pravidlo přesměrování v ukázkovou aplikaci, takže pouze jednu skupinu vložený v řetězci pro nahrazení, který je `$1`.</span><span class="sxs-lookup"><span data-stu-id="48110-196">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="48110-197">Když se pravidlo použije, adresa URL stane `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="48110-197">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="48110-198">Adresa URL přesměrování do zabezpečeného koncového bodu</span><span class="sxs-lookup"><span data-stu-id="48110-198">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="48110-199">Použití `AddRedirectToHttps` pro přesměrování požadavků protokolu HTTP na stejného hostitele a cestu pomocí protokolu HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="48110-199">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="48110-200">Pokud stavový kód není zadán, výchozí hodnota middleware 302 (Found).</span><span class="sxs-lookup"><span data-stu-id="48110-200">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="48110-201">Pokud není port zadaný, middleware použije se výchozí `null`, což znamená, že protokol změn `https://` a klient přistupuje k prostředku na portu 443.</span><span class="sxs-lookup"><span data-stu-id="48110-201">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="48110-202">Tento příklad ukazuje, jak nastavit stavový kód 301 (trvale přesunuto) a změňte port 5001.</span><span class="sxs-lookup"><span data-stu-id="48110-202">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="48110-203">Použití `AddRedirectToHttpsPermanent` přesměrovat nezabezpečené požadavky na stejného hostitele a cestu s zabezpečený protokol HTTPS (`https://` na portu 443).</span><span class="sxs-lookup"><span data-stu-id="48110-203">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="48110-204">Middleware nastaví stavový kód 301 (trvale přesunuto).</span><span class="sxs-lookup"><span data-stu-id="48110-204">The middleware sets the status code to 301 (Moved Permanently).</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="48110-205">Při přesměrování na HTTPS bez nutnosti další přesměrování pravidla, doporučujeme používat Middleware přesměrování protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="48110-205">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="48110-206">Další informace najdete v tématu [vynucení HTTPS](xref:security/enforcing-ssl#require-https) tématu.</span><span class="sxs-lookup"><span data-stu-id="48110-206">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="48110-207">Ukázková aplikace je schopná představením toho, jak používat `AddRedirectToHttps` nebo `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="48110-207">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="48110-208">Přidat metodu rozšíření k `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="48110-208">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="48110-209">Ujistěte se, nezabezpečený požadavek na aplikaci na libovolnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="48110-209">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="48110-210">Zavřít upozornění, že nedůvěryhodný certifikát podepsaný svým držitelem zabezpečení v prohlížeči nebo vytvořte výjimku důvěřovat certifikátům.</span><span class="sxs-lookup"><span data-stu-id="48110-210">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="48110-211">Původní žádost o prostřednictvím `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="48110-211">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="48110-213">Původní žádost o prostřednictvím `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="48110-213">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="48110-215">Přepsání adresy URL</span><span class="sxs-lookup"><span data-stu-id="48110-215">URL rewrite</span></span>

<span data-ttu-id="48110-216">Použití `AddRewrite` k vytvoření pravidla pro přepis adres URL.</span><span class="sxs-lookup"><span data-stu-id="48110-216">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="48110-217">První parametr obsahuje vaše regulárního výrazu pro porovnávání na příchozí cestě adresy URL.</span><span class="sxs-lookup"><span data-stu-id="48110-217">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="48110-218">Druhý parametr je řetězci pro nahrazení.</span><span class="sxs-lookup"><span data-stu-id="48110-218">The second parameter is the replacement string.</span></span> <span data-ttu-id="48110-219">Třetí parametr `skipRemainingRules: {true|false}`, pozná middleware, jestli se mají přeskočit další přepsání pravidla, pokud aktuální pravidlo použije.</span><span class="sxs-lookup"><span data-stu-id="48110-219">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="48110-220">Původní žádosti: `/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="48110-220">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="48110-222">První věc, kterou zjistíte v regulární výraz je stříšky (`^`) na začátku výrazu.</span><span class="sxs-lookup"><span data-stu-id="48110-222">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="48110-223">To znamená, že odpovídající začne na začátku cesty URL.</span><span class="sxs-lookup"><span data-stu-id="48110-223">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="48110-224">V předchozím příkladu se pravidlo přesměrování `redirect-rule/(.*)`, neexistuje žádný ikonu kosočtverce na začátku regulárnímu; proto mohou předcházet žádné znaky `redirect-rule/` v cestě k porovnání.</span><span class="sxs-lookup"><span data-stu-id="48110-224">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="48110-225">Cesta</span><span class="sxs-lookup"><span data-stu-id="48110-225">Path</span></span>                               | <span data-ttu-id="48110-226">Shoda</span><span class="sxs-lookup"><span data-stu-id="48110-226">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="48110-227">Ano</span><span class="sxs-lookup"><span data-stu-id="48110-227">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="48110-228">Ano</span><span class="sxs-lookup"><span data-stu-id="48110-228">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="48110-229">Ano</span><span class="sxs-lookup"><span data-stu-id="48110-229">Yes</span></span>   |

<span data-ttu-id="48110-230">Pravidlo pro přepis adres `^rewrite-rule/(\d+)/(\d+)`, pouze odpovídá cesty, pokud jsou při spuštění systému `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="48110-230">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="48110-231">Všimněte si rozdílu v odpovídající pravidlo pro přepis adres od výše uvedené pravidlo přesměrování.</span><span class="sxs-lookup"><span data-stu-id="48110-231">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="48110-232">Cesta</span><span class="sxs-lookup"><span data-stu-id="48110-232">Path</span></span>                              | <span data-ttu-id="48110-233">Shoda</span><span class="sxs-lookup"><span data-stu-id="48110-233">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="48110-234">Ano</span><span class="sxs-lookup"><span data-stu-id="48110-234">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="48110-235">Ne</span><span class="sxs-lookup"><span data-stu-id="48110-235">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="48110-236">Ne</span><span class="sxs-lookup"><span data-stu-id="48110-236">No</span></span>    |

<span data-ttu-id="48110-237">Následující `^rewrite-rule/` část výrazu, jsou dvě skupiny zachycení `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="48110-237">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="48110-238">`\d` Označuje, že *odpovídá číslici (číslo)*.</span><span class="sxs-lookup"><span data-stu-id="48110-238">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="48110-239">Znaménko plus (`+`) znamená, že *odpovídat nejméně jeden z předchozí znak*.</span><span class="sxs-lookup"><span data-stu-id="48110-239">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="48110-240">Proto se adresa URL musí obsahovat číslo za nímž následuje lomítko následované jiné číslo.</span><span class="sxs-lookup"><span data-stu-id="48110-240">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="48110-241">Tyto zachytávání skupiny jsou vloženy do přepsaný adresy URL jako `$1` a `$2`.</span><span class="sxs-lookup"><span data-stu-id="48110-241">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="48110-242">Přepište řetězci pro nahrazení pravidlo umístí zachycené skupiny do řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="48110-242">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="48110-243">Požadovaná cesta `/rewrite-rule/1234/5678` je přepsán získat prostředek na `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="48110-243">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="48110-244">Pokud dotazu je k dispozici na původní požadavek, to se zachová, i když adresa URL je přepsán.</span><span class="sxs-lookup"><span data-stu-id="48110-244">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="48110-245">Neexistuje žádná zpětná transformace na server se získat prostředek.</span><span class="sxs-lookup"><span data-stu-id="48110-245">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="48110-246">Pokud existuje prostředek má načíst a vrácen do klienta s kódem stavový kód 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="48110-246">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="48110-247">Protože klient není přesměrován, adresy URL v adresním řádku prohlížeče se nezmění.</span><span class="sxs-lookup"><span data-stu-id="48110-247">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="48110-248">Jak jde klienta, nikdy operace přepisování adres URL došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="48110-248">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="48110-249">Použití `skipRemainingRules: true` kdykoli je to možné, protože pravidel je nákladný proces a zkracuje dobu odezvy aplikace.</span><span class="sxs-lookup"><span data-stu-id="48110-249">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="48110-250">Nejrychlejší odezvu aplikace:</span><span class="sxs-lookup"><span data-stu-id="48110-250">For the fastest app response:</span></span>
> * <span data-ttu-id="48110-251">Pořadí přepsání pravidel z nejčastěji odpovídající pravidla nejméně často odpovídající pravidla.</span><span class="sxs-lookup"><span data-stu-id="48110-251">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="48110-252">Zpracování zbývající pravidla přeskočte, pokud je nalezena shoda, a je potřeba žádná další pravidla zpracování.</span><span class="sxs-lookup"><span data-stu-id="48110-252">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="48110-253">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="48110-253">Apache mod_rewrite</span></span>

<span data-ttu-id="48110-254">Použití Apache mod_rewrite pravidel s `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="48110-254">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="48110-255">Ujistěte se, že soubor pravidel je nasazen s aplikací.</span><span class="sxs-lookup"><span data-stu-id="48110-255">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="48110-256">Další informace a příklady pravidel mod_rewrite najdete v tématu [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="48110-256">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="48110-257">A `StreamReader` slouží k načtení pravidla z *ApacheModRewrite.txt* soubor pravidel.</span><span class="sxs-lookup"><span data-stu-id="48110-257">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="48110-258">První parametr přebírá `IFileProvider`, která se poskytuje prostřednictvím [injektáž závislostí](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="48110-258">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="48110-259">`IHostingEnvironment` Se vloží poskytnout `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="48110-259">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="48110-260">Druhý parametr je cesta k souboru pravidel, která je *ApacheModRewrite.txt* v ukázkové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="48110-260">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="48110-261">Ukázková aplikace přesměruje požadavky z `/apache-mod-rules-redirect/(.\*)` k `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="48110-261">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="48110-262">Je stavový kód odpovědi 302 (Found).</span><span class="sxs-lookup"><span data-stu-id="48110-262">The response status code is 302 (Found).</span></span>

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

<span data-ttu-id="48110-263">Původní žádosti: `/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="48110-263">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí](url-rewriting/_static/add_apache_mod_redirect.png)

<span data-ttu-id="48110-265">Middleware podporuje následující proměnné na serveru Apache mod_rewrite:</span><span class="sxs-lookup"><span data-stu-id="48110-265">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="48110-266">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="48110-266">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="48110-267">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="48110-267">HTTP_ACCEPT</span></span>
* <span data-ttu-id="48110-268">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="48110-268">HTTP_CONNECTION</span></span>
* <span data-ttu-id="48110-269">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="48110-269">HTTP_COOKIE</span></span>
* <span data-ttu-id="48110-270">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="48110-270">HTTP_FORWARDED</span></span>
* <span data-ttu-id="48110-271">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="48110-271">HTTP_HOST</span></span>
* <span data-ttu-id="48110-272">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="48110-272">HTTP_REFERER</span></span>
* <span data-ttu-id="48110-273">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="48110-273">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="48110-274">HTTPS</span><span class="sxs-lookup"><span data-stu-id="48110-274">HTTPS</span></span>
* <span data-ttu-id="48110-275">IPV6</span><span class="sxs-lookup"><span data-stu-id="48110-275">IPV6</span></span>
* <span data-ttu-id="48110-276">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="48110-276">QUERY_STRING</span></span>
* <span data-ttu-id="48110-277">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="48110-277">REMOTE_ADDR</span></span>
* <span data-ttu-id="48110-278">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="48110-278">REMOTE_PORT</span></span>
* <span data-ttu-id="48110-279">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="48110-279">REQUEST_FILENAME</span></span>
* <span data-ttu-id="48110-280">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="48110-280">REQUEST_METHOD</span></span>
* <span data-ttu-id="48110-281">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="48110-281">REQUEST_SCHEME</span></span>
* <span data-ttu-id="48110-282">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="48110-282">REQUEST_URI</span></span>
* <span data-ttu-id="48110-283">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="48110-283">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="48110-284">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="48110-284">SERVER_ADDR</span></span>
* <span data-ttu-id="48110-285">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="48110-285">SERVER_PORT</span></span>
* <span data-ttu-id="48110-286">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="48110-286">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="48110-287">ČAS</span><span class="sxs-lookup"><span data-stu-id="48110-287">TIME</span></span>
* <span data-ttu-id="48110-288">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="48110-288">TIME_DAY</span></span>
* <span data-ttu-id="48110-289">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="48110-289">TIME_HOUR</span></span>
* <span data-ttu-id="48110-290">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="48110-290">TIME_MIN</span></span>
* <span data-ttu-id="48110-291">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="48110-291">TIME_MON</span></span>
* <span data-ttu-id="48110-292">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="48110-292">TIME_SEC</span></span>
* <span data-ttu-id="48110-293">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="48110-293">TIME_WDAY</span></span>
* <span data-ttu-id="48110-294">MODULU KPI</span><span class="sxs-lookup"><span data-stu-id="48110-294">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="48110-295">Modul přepisování adres URL služby IIS pravidla</span><span class="sxs-lookup"><span data-stu-id="48110-295">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="48110-296">Pokud chcete použít pravidla, která platí pro modul přepisování adres URL služby IIS, použijte `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="48110-296">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="48110-297">Ujistěte se, že soubor pravidel je nasazen s aplikací.</span><span class="sxs-lookup"><span data-stu-id="48110-297">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="48110-298">Není přímá middlewaru, který má být použit vaše *web.config* souboru při spuštění ve Windows serveru služby IIS.</span><span class="sxs-lookup"><span data-stu-id="48110-298">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="48110-299">Se službou IIS, tato pravidla by měly být uloženy mimo vaši *web.config* aby nedocházelo ke konfliktům s modulem IIS Rewrite.</span><span class="sxs-lookup"><span data-stu-id="48110-299">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="48110-300">Další informace a příklady pravidel modul přepisování adres URL služby IIS najdete v tématu [pomocí přepisování adres Url modul 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) a [URL přepsání konfigurace odkazu na modul](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="48110-300">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="48110-301">A `StreamReader` slouží k načtení pravidla z *IISUrlRewrite.xml* soubor pravidel.</span><span class="sxs-lookup"><span data-stu-id="48110-301">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="48110-302">První parametr přebírá `IFileProvider`, zatímco druhý parametr je cesta k souboru XML pravidla, která je *IISUrlRewrite.xml* v ukázkové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="48110-302">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="48110-303">Ukázková aplikace přepíše žádosti od `/iis-rules-rewrite/(.*)` k `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="48110-303">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="48110-304">Odpověď je odeslána na klienta se stavovým kódem 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="48110-304">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

<span data-ttu-id="48110-305">Původní žádosti: `/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="48110-305">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="48110-307">Pokud máte aktivní revize modul IIS s nakonfigurovaná pravidla úrovni serveru, které by ovlivnily aplikace nežádoucí způsoby, můžete zakázat modul IIS revize pro danou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="48110-307">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="48110-308">Další informace najdete v tématu [moduly IIS zakázání](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="48110-308">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="48110-309">Nepodporované funkce</span><span class="sxs-lookup"><span data-stu-id="48110-309">Unsupported features</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="48110-310">Middleware vydané s ASP.NET Core 2.x nepodporuje následující funkce modul přepisování adres URL služby IIS:</span><span class="sxs-lookup"><span data-stu-id="48110-310">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="48110-311">Odchozí pravidla</span><span class="sxs-lookup"><span data-stu-id="48110-311">Outbound Rules</span></span>
* <span data-ttu-id="48110-312">Vlastní serverové proměnné</span><span class="sxs-lookup"><span data-stu-id="48110-312">Custom Server Variables</span></span>
* <span data-ttu-id="48110-313">Zástupné znaky</span><span class="sxs-lookup"><span data-stu-id="48110-313">Wildcards</span></span>
* <span data-ttu-id="48110-314">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="48110-314">LogRewrittenUrl</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="48110-315">Middleware vydané s ASP.NET Core 1.x nepodporuje následující funkce modul přepisování adres URL služby IIS:</span><span class="sxs-lookup"><span data-stu-id="48110-315">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="48110-316">Globální pravidla</span><span class="sxs-lookup"><span data-stu-id="48110-316">Global Rules</span></span>
* <span data-ttu-id="48110-317">Odchozí pravidla</span><span class="sxs-lookup"><span data-stu-id="48110-317">Outbound Rules</span></span>
* <span data-ttu-id="48110-318">Přepište mapy</span><span class="sxs-lookup"><span data-stu-id="48110-318">Rewrite Maps</span></span>
* <span data-ttu-id="48110-319">CustomResponse akce</span><span class="sxs-lookup"><span data-stu-id="48110-319">CustomResponse Action</span></span>
* <span data-ttu-id="48110-320">Vlastní serverové proměnné</span><span class="sxs-lookup"><span data-stu-id="48110-320">Custom Server Variables</span></span>
* <span data-ttu-id="48110-321">Zástupné znaky</span><span class="sxs-lookup"><span data-stu-id="48110-321">Wildcards</span></span>
* <span data-ttu-id="48110-322">Akce: CustomResponse</span><span class="sxs-lookup"><span data-stu-id="48110-322">Action:CustomResponse</span></span>
* <span data-ttu-id="48110-323">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="48110-323">LogRewrittenUrl</span></span>

::: moniker-end

#### <a name="supported-server-variables"></a><span data-ttu-id="48110-324">Podporované serverové proměnné</span><span class="sxs-lookup"><span data-stu-id="48110-324">Supported server variables</span></span>

<span data-ttu-id="48110-325">Middleware podporuje následující proměnné serveru modul přepisování adres URL služby IIS:</span><span class="sxs-lookup"><span data-stu-id="48110-325">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="48110-326">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="48110-326">CONTENT_LENGTH</span></span>
* <span data-ttu-id="48110-327">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="48110-327">CONTENT_TYPE</span></span>
* <span data-ttu-id="48110-328">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="48110-328">HTTP_ACCEPT</span></span>
* <span data-ttu-id="48110-329">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="48110-329">HTTP_CONNECTION</span></span>
* <span data-ttu-id="48110-330">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="48110-330">HTTP_COOKIE</span></span>
* <span data-ttu-id="48110-331">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="48110-331">HTTP_HOST</span></span>
* <span data-ttu-id="48110-332">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="48110-332">HTTP_REFERER</span></span>
* <span data-ttu-id="48110-333">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="48110-333">HTTP_URL</span></span>
* <span data-ttu-id="48110-334">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="48110-334">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="48110-335">HTTPS</span><span class="sxs-lookup"><span data-stu-id="48110-335">HTTPS</span></span>
* <span data-ttu-id="48110-336">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="48110-336">LOCAL_ADDR</span></span>
* <span data-ttu-id="48110-337">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="48110-337">QUERY_STRING</span></span>
* <span data-ttu-id="48110-338">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="48110-338">REMOTE_ADDR</span></span>
* <span data-ttu-id="48110-339">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="48110-339">REMOTE_PORT</span></span>
* <span data-ttu-id="48110-340">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="48110-340">REQUEST_FILENAME</span></span>
* <span data-ttu-id="48110-341">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="48110-341">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="48110-342">Můžete také získat `IFileProvider` prostřednictvím `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="48110-342">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="48110-343">Tento přístup může poskytnout větší flexibilitu pro umístění vaší revize souborů pravidel.</span><span class="sxs-lookup"><span data-stu-id="48110-343">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="48110-344">Ujistěte se, že soubory přepsání pravidel jsou nasazeny na server v cestě, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="48110-344">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="48110-345">Pravidlo na základě – metoda</span><span class="sxs-lookup"><span data-stu-id="48110-345">Method-based rule</span></span>

<span data-ttu-id="48110-346">Použití `Add(Action<RewriteContext> applyRule)` implementovat vlastní logiku pravidla v metodě.</span><span class="sxs-lookup"><span data-stu-id="48110-346">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="48110-347">`RewriteContext` Zpřístupňuje `HttpContext` pro použití v metodě.</span><span class="sxs-lookup"><span data-stu-id="48110-347">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="48110-348">`RewriteContext.Result` Určuje, jak další kanálu probíhá zpracování.</span><span class="sxs-lookup"><span data-stu-id="48110-348">The `RewriteContext.Result` determines how additional pipeline processing is handled.</span></span>

| `RewriteContext.Result`              | <span data-ttu-id="48110-349">Akce</span><span class="sxs-lookup"><span data-stu-id="48110-349">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="48110-350">`RuleResult.ContinueRules` (výchozí)</span><span class="sxs-lookup"><span data-stu-id="48110-350">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="48110-351">Pokračovat v použití pravidel</span><span class="sxs-lookup"><span data-stu-id="48110-351">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="48110-352">Zastavit použití pravidel a odeslání odpovědi</span><span class="sxs-lookup"><span data-stu-id="48110-352">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="48110-353">Zastavit použití pravidel a odeslat kontextu do dalšího middlewaru</span><span class="sxs-lookup"><span data-stu-id="48110-353">Stop applying rules and send the context to the next middleware</span></span> |

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="48110-354">Ukázková aplikace předvádí metodu, který přesměruje požadavky pro cesty, která končit *.xml*.</span><span class="sxs-lookup"><span data-stu-id="48110-354">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="48110-355">Pokud vytvoříte žádost o `/file.xml`, přesměruje se na `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="48110-355">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="48110-356">Stavový kód je nastaven na 301 (trvale přesunuto).</span><span class="sxs-lookup"><span data-stu-id="48110-356">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="48110-357">Pro přesměrování je potřeba explicitně nastavit stavový kód odpovědi; v opačném případě se vrátí stavový kód 200 (OK) a přesměrování nedojde na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="48110-357">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

<span data-ttu-id="48110-358">Původní žádosti: `/file.xml`</span><span class="sxs-lookup"><span data-stu-id="48110-358">Original Request: `/file.xml`</span></span>

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí pro file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="48110-360">Pravidlo na základě IRule</span><span class="sxs-lookup"><span data-stu-id="48110-360">IRule-based rule</span></span>

<span data-ttu-id="48110-361">Použití `Add(IRule)` k zapouzdření pravidlo logiku do třídy, která implementuje `IRule` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="48110-361">Use `Add(IRule)` to encapsulate your own rule logic in a class that implements the `IRule` interface.</span></span> <span data-ttu-id="48110-362">Pomocí `IRule` nabízí větší flexibilitu v porovnání s využitím pravidel založených na volání metody přístupu.</span><span class="sxs-lookup"><span data-stu-id="48110-362">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="48110-363">Vaší společnosti jednotně uplatňovat třídy může obsahovat konstruktor, kde můžete předat parametry `ApplyRule` metody.</span><span class="sxs-lookup"><span data-stu-id="48110-363">Your implementaion class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="48110-364">Hodnoty parametrů v ukázkové aplikaci pro `extension` a `newPath` jsou kontrolovány pro splnění určitých podmínek.</span><span class="sxs-lookup"><span data-stu-id="48110-364">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="48110-365">`extension` Musí obsahovat hodnotu, a hodnota musí být *.png*, *.jpg*, nebo *.gif*.</span><span class="sxs-lookup"><span data-stu-id="48110-365">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="48110-366">Pokud `newPath` není platný, `ArgumentException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="48110-366">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="48110-367">Pokud vytvoříte žádost o *image.png*, přesměruje se na `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="48110-367">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="48110-368">Pokud vytvoříte žádost o *image.jpg*, přesměruje se na `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="48110-368">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="48110-369">Kód stavu je nastaven na 301 (trvale přesunuto) a `context.Result` je nastavena na zastavení zpracování pravidel a odeslání odpovědi.</span><span class="sxs-lookup"><span data-stu-id="48110-369">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

<span data-ttu-id="48110-370">Původní žádosti: `/image.png`</span><span class="sxs-lookup"><span data-stu-id="48110-370">Original Request: `/image.png`</span></span>

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí pro image.png](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="48110-372">Původní žádosti: `/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="48110-372">Original Request: `/image.jpg`</span></span>

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí pro image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="48110-374">Příklady regulární výraz</span><span class="sxs-lookup"><span data-stu-id="48110-374">Regex examples</span></span>

| <span data-ttu-id="48110-375">Cíl</span><span class="sxs-lookup"><span data-stu-id="48110-375">Goal</span></span> | <span data-ttu-id="48110-376">Řetězec regulárního výrazu &</span><span class="sxs-lookup"><span data-stu-id="48110-376">Regex String &</span></span><br><span data-ttu-id="48110-377">Příklad shody</span><span class="sxs-lookup"><span data-stu-id="48110-377">Match Example</span></span> | <span data-ttu-id="48110-378">Náhradní řetězec &</span><span class="sxs-lookup"><span data-stu-id="48110-378">Replacement String &</span></span><br><span data-ttu-id="48110-379">Příklad výstupu</span><span class="sxs-lookup"><span data-stu-id="48110-379">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="48110-380">Přepsat cestu do řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="48110-380">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="48110-381">Odstranit koncové lomítko</span><span class="sxs-lookup"><span data-stu-id="48110-381">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="48110-382">Vynucení adresy koncové lomítko</span><span class="sxs-lookup"><span data-stu-id="48110-382">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="48110-383">Vyhněte se přepsání určité žádosti</span><span class="sxs-lookup"><span data-stu-id="48110-383">Avoid rewriting specific requests</span></span> | <span data-ttu-id="48110-384">`^(.*)(?<!\.axd)$` Nebo `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="48110-384">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="48110-385">Ano: `/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="48110-385">Yes: `/resource.htm`</span></span><br><span data-ttu-id="48110-386">Ne: `/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="48110-386">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="48110-387">Změna uspořádání segmenty adres URL</span><span class="sxs-lookup"><span data-stu-id="48110-387">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="48110-388">Nahraďte segment adresy URL</span><span class="sxs-lookup"><span data-stu-id="48110-388">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="48110-389">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="48110-389">Additional resources</span></span>

* [<span data-ttu-id="48110-390">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="48110-390">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="48110-391">Middleware</span><span class="sxs-lookup"><span data-stu-id="48110-391">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="48110-392">Regulární výrazy v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="48110-392">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="48110-393">Jazyk regulárních výrazů – Stručná referenční příručka</span><span class="sxs-lookup"><span data-stu-id="48110-393">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="48110-394">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="48110-394">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="48110-395">Pomocí modulu přepisování adres Url 2.0 (pro IIS)</span><span class="sxs-lookup"><span data-stu-id="48110-395">Using Url Rewrite Module 2.0 (for IIS)</span></span>](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="48110-396">Odkaz na modul konfigurace adresy URL revize</span><span class="sxs-lookup"><span data-stu-id="48110-396">URL Rewrite Module Configuration Reference</span></span>](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="48110-397">Fórum modulu přepsání adresy URL služby IIS</span><span class="sxs-lookup"><span data-stu-id="48110-397">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="48110-398">Zachovat jednoduchá struktura adresy URL</span><span class="sxs-lookup"><span data-stu-id="48110-398">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="48110-399">10 přepisování adres URL tipy a triky</span><span class="sxs-lookup"><span data-stu-id="48110-399">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="48110-400">Lomítko nebo lomítko</span><span class="sxs-lookup"><span data-stu-id="48110-400">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
