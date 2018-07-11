---
title: Zpracování chyb v ASP.NET Core
author: ardalis
description: Objevte, jak zpracovávat chyby v aplikacích ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/05/2018
uid: fundamentals/error-handling
ms.openlocfilehash: 6aded9525a0abd31dec8441c7fba60d8845c7d93
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938238"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="48008-103">Zpracování chyb v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="48008-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="48008-104">Podle [Steve Smith](https://ardalis.com/) a [Petr Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="48008-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="48008-105">Tento článek se věnuje běžné přístupy k zpracování chyb v aplikacích ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="48008-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="48008-106">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="48008-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="48008-107">Stránce výjimek pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="48008-107">The developer exception page</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="48008-108">Chcete-li nakonfigurovat aplikaci, která zobrazí stránka, která jsou uvedeny podrobné informace o výjimkách, použijte *stránku výjimek pro vývojáře*.</span><span class="sxs-lookup"><span data-stu-id="48008-108">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="48008-109">Stránka k dispozici ve [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) balíček, který je k dispozici v [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="48008-109">The page made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="48008-110">Přidejte řádek, který `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="48008-110">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="48008-111">Chcete-li nakonfigurovat aplikaci, která zobrazí stránka, která jsou uvedeny podrobné informace o výjimkách, použijte *stránku výjimek pro vývojáře*.</span><span class="sxs-lookup"><span data-stu-id="48008-111">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="48008-112">Na stránce je k dispozici ve [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) balíček, který je k dispozici v [metabalíček Microsoft.aspnetcore.all](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="48008-112">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="48008-113">Přidejte řádek, který `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="48008-113">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="48008-114">Chcete-li nakonfigurovat aplikaci, která zobrazí stránka, která jsou uvedeny podrobné informace o výjimkách, použijte *stránku výjimek pro vývojáře*.</span><span class="sxs-lookup"><span data-stu-id="48008-114">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="48008-115">Na stránce je k dispozici tak, že přidáte odkaz na balíček pro [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) balíčku v souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="48008-115">The page is made available by adding a package reference for the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package in the project file.</span></span> <span data-ttu-id="48008-116">Přidejte řádek, který `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="48008-116">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="48008-117">Umístěte volání [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) před veškerý middleware, ve které chcete zaznamenat tak výjimky, jako například `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="48008-117">Place the call to [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) in front of any middleware where you want to catch exceptions, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="48008-118">Povolit na stránce výjimek pro vývojáře **pouze, když je aplikace spuštěna ve vývojovém prostředí**.</span><span class="sxs-lookup"><span data-stu-id="48008-118">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="48008-119">Nechcete veřejně sdílet podrobné informace o výjimce při spuštění aplikace v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="48008-119">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="48008-120">[Další informace o konfiguraci prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="48008-120">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="48008-121">Stránce výjimek pro vývojáře najdete spuštění ukázkové aplikace s prostředím nastavena na `Development`a přidejte `?throw=true` k základní adrese URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="48008-121">To see the developer exception page, run the sample app with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="48008-122">Stránka obsahuje několik karet s informacemi o výjimku a požadavek.</span><span class="sxs-lookup"><span data-stu-id="48008-122">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="48008-123">První karta obsahuje trasování zásobníku:</span><span class="sxs-lookup"><span data-stu-id="48008-123">The first tab includes a stack trace:</span></span>

![Trasování zásobníku](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="48008-125">Další karta ukazuje dotaz případné parametry řetězce:</span><span class="sxs-lookup"><span data-stu-id="48008-125">The next tab shows the query string parameters, if any:</span></span>

![Parametry řetězce dotazu](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="48008-127">Pokud požadavek má soubory cookie, zobrazí se na **soubory cookie** kartu. Záhlaví se zobrazují na kartě poslední:</span><span class="sxs-lookup"><span data-stu-id="48008-127">If the request has cookies, they appear on the **Cookies** tab. Headers are seen in the last tab:</span></span>

![Záhlaví](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="48008-129">Konfigurace vlastní výjimky zpracování stránky</span><span class="sxs-lookup"><span data-stu-id="48008-129">Configuring a custom exception handling page</span></span>

<span data-ttu-id="48008-130">Na stránce obslužné rutiny výjimky pro použití při není aplikace spuštěna konfiguraci `Development` prostředí:</span><span class="sxs-lookup"><span data-stu-id="48008-130">Configure an exception handler page to use when the app isn't running in the `Development` environment:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="48008-131">V aplikaci s Razor Pages [dotnet nové](/dotnet/core/tools/dotnet-new) chybová stránka obsahuje šablona Razor Pages a `ErrorModel` stránce třídy modelu v *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="48008-131">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and `ErrorModel` page model class in the *Pages* folder.</span></span>

<span data-ttu-id="48008-132">V aplikaci MVC, není uspořádání metody akce obslužná rutina chyby s atributy metody HTTP, jako například `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="48008-132">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="48008-133">Explicitní příkazy zabránit v dosažení metodu některé požadavky.</span><span class="sxs-lookup"><span data-stu-id="48008-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="48008-134">Povolit anonymní přístup k metodě tak, aby se neověřené uživatele dostávají zobrazení chyb.</span><span class="sxs-lookup"><span data-stu-id="48008-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="48008-135">Například poskytuje následující metody obslužné rutiny chyb [dotnet nové](/dotnet/core/tools/dotnet-new) šablona MVC a zobrazí se v kontroler Home:</span><span class="sxs-lookup"><span data-stu-id="48008-135">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="48008-136">Konfigurace stavu znakové stránky</span><span class="sxs-lookup"><span data-stu-id="48008-136">Configuring status code pages</span></span>

<span data-ttu-id="48008-137">Ve výchozím nastavení, aplikace neposkytuje znakovou stránku bohaté stav pro stavové kódy HTTP, jako například *404 Nenalezeno*.</span><span class="sxs-lookup"><span data-stu-id="48008-137">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="48008-138">Poskytnout stav znakových stránek, nakonfigurujte Middleware stránky kód stavu přidáním čáry `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="48008-138">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="48008-139">Ve výchozím nastavení přidá Middleware stránky stavový kód obslužné rutiny pro běžné stavové kódy, jako je například 404 prostého textu:</span><span class="sxs-lookup"><span data-stu-id="48008-139">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as 404:</span></span>

![404 – Stránka](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="48008-141">Middleware podporuje několik metod rozšíření.</span><span class="sxs-lookup"><span data-stu-id="48008-141">The middleware supports several extension methods.</span></span> <span data-ttu-id="48008-142">Jedna metoda má výraz lambda:</span><span class="sxs-lookup"><span data-stu-id="48008-142">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="48008-143">Jinou metodou přijímá řetězec typ a formát obsahu:</span><span class="sxs-lookup"><span data-stu-id="48008-143">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="48008-144">Je také přesměrovat a znovu provést metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="48008-144">There's also redirect and re-execute extension methods.</span></span> <span data-ttu-id="48008-145">Metoda přesměrování odesílá *302 Found* stavový kód na straně klienta:</span><span class="sxs-lookup"><span data-stu-id="48008-145">The redirect method sends a *302 Found* status code to the client:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="48008-146">Znovu spustit metodu vrátí původní stavový kód klienta ale také spustí obslužnou rutinu pro adresu URL přesměrování:</span><span class="sxs-lookup"><span data-stu-id="48008-146">The re-execute method returns the original status code to the client but also executes the handler for the redirect URL:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="48008-147">Stav znakové stránky je možné zakázat pro konkrétní požadavky v metodě obslužné rutiny pro stránky Razor nebo kontroler MVC.</span><span class="sxs-lookup"><span data-stu-id="48008-147">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="48008-148">Zakázat stav znakových stránek, pokus o načtení [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) z identifikátoru požadavku [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) kolekce a zakažte funkci, pokud je k dispozici:</span><span class="sxs-lookup"><span data-stu-id="48008-148">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="48008-149">Pokud se používá `UseStatusCodePages*` přetížení body do koncového bodu v rámci aplikace, vytvořte zobrazení MVC nebo stránky Razor pro koncový bod.</span><span class="sxs-lookup"><span data-stu-id="48008-149">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="48008-150">Například [dotnet nové](/dotnet/core/tools/dotnet-new) šablony pro aplikace Razor Pages vytvoří následující stránky a modelu třídy stránky:</span><span class="sxs-lookup"><span data-stu-id="48008-150">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="48008-151">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="48008-151">*Error.cshtml*:</span></span>

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

<span data-ttu-id="48008-152">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="48008-152">*Error.cshtml.cs*:</span></span>

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="48008-153">Kód zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="48008-153">Exception-handling code</span></span>

<span data-ttu-id="48008-154">Kód výjimky zpracování stránky může vyvolat výjimky.</span><span class="sxs-lookup"><span data-stu-id="48008-154">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="48008-155">Často je vhodné pro produkční chybové stránky, které se skládají z čistě statický obsah.</span><span class="sxs-lookup"><span data-stu-id="48008-155">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="48008-156">Také mějte na paměti, že po odeslání hlaviček pro odpovědi, nelze změnit stavový kód odpovědi ani žádné výjimce stránky nebo obslužné rutiny poběží.</span><span class="sxs-lookup"><span data-stu-id="48008-156">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="48008-157">Odpovědi musí dokončit, nebo bylo připojení přerušeno.</span><span class="sxs-lookup"><span data-stu-id="48008-157">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="48008-158">Zpracování výjimek serveru</span><span class="sxs-lookup"><span data-stu-id="48008-158">Server exception handling</span></span>

<span data-ttu-id="48008-159">Kromě ve vaší aplikaci logiky zpracování výjimek [server](xref:fundamentals/servers/index) hostování vaší aplikace provádí nějaké zpracování výjimek.</span><span class="sxs-lookup"><span data-stu-id="48008-159">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="48008-160">Pokud server zachytí výjimku, před odesláním záhlaví, odešle server *500 – Interní chyba serveru* odpověď se žádný text.</span><span class="sxs-lookup"><span data-stu-id="48008-160">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="48008-161">Pokud server zachytí výjimku po odeslání hlavičky, server ukončí připojení.</span><span class="sxs-lookup"><span data-stu-id="48008-161">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="48008-162">Požadavky, které nejsou zpracovány aplikací jsou zpracovány serverem.</span><span class="sxs-lookup"><span data-stu-id="48008-162">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="48008-163">Všechny vyvolané výjimky jsou zpracována výjimka serveru zpracování.</span><span class="sxs-lookup"><span data-stu-id="48008-163">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="48008-164">Žádné nakonfigurované vlastní chybové stránky nebo zpracování výjimek middleware nebo filtry nemají vliv na toto chování.</span><span class="sxs-lookup"><span data-stu-id="48008-164">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="48008-165">Zpracování výjimek při spuštění</span><span class="sxs-lookup"><span data-stu-id="48008-165">Startup exception handling</span></span>

<span data-ttu-id="48008-166">Pouze hostování vrstvy dokáže zpracovat výjimky, které se provedou při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="48008-166">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="48008-167">Použití [webového hostitele](xref:fundamentals/host/web-host), můžete [konfigurace hostitele chování v reakci na chyby při spuštění](xref:fundamentals/host/web-host#detailed-errors) s `captureStartupErrors` a `detailedErrors` klíče.</span><span class="sxs-lookup"><span data-stu-id="48008-167">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="48008-168">Hostování můžete jenom zobrazit chybovou stránku pro chyby zaznamenané při spouštění, pokud dojde k chybě po adresa/port hostitele vazby.</span><span class="sxs-lookup"><span data-stu-id="48008-168">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="48008-169">Pokud z nějakého důvodu selže všechny vazby, hostování vrstvy zaznamená kritické výjimky, dojde k chybě dotnet procesu a žádná chybová stránka se zobrazí, když je aplikace spuštěná na [Kestrel](xref:fundamentals/servers/kestrel) serveru.</span><span class="sxs-lookup"><span data-stu-id="48008-169">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="48008-170">Při spuštění na [IIS](/iis) nebo [služby IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 selhání procesu* je vrácený [modul ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) Pokud proces nemůže být bylo zahájeno.</span><span class="sxs-lookup"><span data-stu-id="48008-170">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="48008-171">Informace o řešení problémů se spouštěním, při hostování za nástrojem službou IIS najdete v tématu <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="48008-171">For information on troubleshooting startup issues when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="48008-172">Informace o řešení problémů se spouštěním pomocí služby Azure App Service najdete v tématu <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="48008-172">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="48008-173">Zpracování chyb v ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="48008-173">ASP.NET MVC error handling</span></span>

<span data-ttu-id="48008-174">[MVC](xref:mvc/overview) aplikace mají některé další možnosti pro zpracování chyb, jako je například konfigurace filtry výjimek a provedení ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="48008-174">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="48008-175">Filtry výjimek</span><span class="sxs-lookup"><span data-stu-id="48008-175">Exception filters</span></span>

<span data-ttu-id="48008-176">Filtry výjimek se dá nakonfigurovat globálně nebo na základě na kontroler nebo akcích v aplikaci MVC.</span><span class="sxs-lookup"><span data-stu-id="48008-176">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="48008-177">Tyto filtry zpracování neošetřené výjimky, která během provádění akce kontroleru nebo jiný filtr.</span><span class="sxs-lookup"><span data-stu-id="48008-177">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="48008-178">Tyto filtry nejsou volány jinak.</span><span class="sxs-lookup"><span data-stu-id="48008-178">These filters aren't called otherwise.</span></span> <span data-ttu-id="48008-179">Další informace najdete v tématu [filtry](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="48008-179">To learn more, see [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="48008-180">Filtry výjimek jsou vhodné pro soutisku výjimky, ke kterým dochází v rámci akce MVC, ale nejsou tak flexibilní jako middleware pro zpracování chyb.</span><span class="sxs-lookup"><span data-stu-id="48008-180">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="48008-181">Obecně přednost použití middlewaru a použití filtrů jenom tam, kde potřebujete provádět zpracování chyb *jinak* závislosti na zvolené akci, která MVC.</span><span class="sxs-lookup"><span data-stu-id="48008-181">Generally prefer the use of middleware, and use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="48008-182">Zpracování chyby stavu modelu</span><span class="sxs-lookup"><span data-stu-id="48008-182">Handling model state errors</span></span>

<span data-ttu-id="48008-183">[Ověření modelu](xref:mvc/models/validation) vyvolá se před vyvoláním každé akce kontroleru a zodpovídá za metodu akce ke kontrole `ModelState.IsValid` a reagují odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="48008-183">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="48008-184">Některé aplikace zvolte dodržovat standardní konvence týkající se chyby ověření modelu, v takovém případě [filtr](xref:mvc/controllers/filters) může být vhodné místo pro implementaci tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="48008-184">Some apps choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="48008-185">Měli byste otestovat chování vaše akce se stavy modelu je neplatný.</span><span class="sxs-lookup"><span data-stu-id="48008-185">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="48008-186">Další informace najdete v [testovací kontroler logiku](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="48008-186">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48008-187">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="48008-187">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
