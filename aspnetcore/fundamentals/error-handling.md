---
title: Zpracování chyb v ASP.NET Core
author: ardalis
description: Můžete zjistit, jak se budou zpracovávat chyby v aplikacích ASP.NET Core.
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
uid: fundamentals/error-handling
ms.openlocfilehash: 2fe46ecc32d61a7fafb2ad6e2a35456476608251
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273706"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="a0167-103">Zpracování chyb v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0167-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="a0167-104">Podle [Steve Smith](https://ardalis.com/) a [tní Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="a0167-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="a0167-105">Tento článek popisuje běžné appoaches pro zpracování chyb v aplikacích ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a0167-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="a0167-106">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a0167-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="a0167-107">Stránka výjimka vývojáře</span><span class="sxs-lookup"><span data-stu-id="a0167-107">The developer exception page</span></span>

<span data-ttu-id="a0167-108">Konfigurace aplikace pro zobrazení stránky, které jsou uvedeny podrobné informace o výjimkách, nainstalujte `Microsoft.AspNetCore.Diagnostics` NuGet balíček a přidat čáru, která [konfigurace metody ve třídě spuštění](xref:fundamentals/startup):</span><span class="sxs-lookup"><span data-stu-id="a0167-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](xref:fundamentals/startup):</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="a0167-109">Uveďte `UseDeveloperExceptionPage` před veškerý middleware chcete zachytit výjimky, jako například `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="a0167-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="a0167-110">Povolit výjimky stránce vývojáře **jenom když aplikace běží ve vývojovém prostředí**.</span><span class="sxs-lookup"><span data-stu-id="a0167-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="a0167-111">Nechcete sdílet informace podrobné výjimky veřejně při spuštění aplikace v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a0167-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="a0167-112">[Další informace o konfiguraci prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="a0167-112">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="a0167-113">Pokud chcete zobrazit stránku výjimka vývojáře, spusťte ukázkovou aplikaci s prostředím nastavena na `Development`a přidejte `?throw=true` pro základní adresu URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0167-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="a0167-114">Stránka obsahuje několik karet s informacemi o výjimku a požadavek.</span><span class="sxs-lookup"><span data-stu-id="a0167-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="a0167-115">Na první kartě zahrnuje trasování zásobníku.</span><span class="sxs-lookup"><span data-stu-id="a0167-115">The first tab includes a stack trace.</span></span> 

![Trasování zásobníku](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="a0167-117">Další karta ukazuje dotaz parametrů řetězce, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="a0167-117">The next tab shows the query string parameters, if any.</span></span>

![Parametrů řetězce dotazu](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="a0167-119">Tento požadavek nebyly k dispozici žádné soubory cookie, ale pokud neodpovídala, by se na **soubory cookie** kartě. Můžete zobrazit seznam hlaviček, které byly předány na kartě poslední.</span><span class="sxs-lookup"><span data-stu-id="a0167-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Záhlaví](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="a0167-121">Konfigurace vlastní výjimky zpracování stránky</span><span class="sxs-lookup"><span data-stu-id="a0167-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="a0167-122">Nakonfigurovat stránku obslužná rutina výjimky pro použití při není aplikace spuštěna `Development` prostředí.</span><span class="sxs-lookup"><span data-stu-id="a0167-122">Configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="a0167-123">V aplikaci pro stránky Razor [dotnet nové](/dotnet/core/tools/dotnet-new) šablona stránky Razor poskytuje chybovou stránku a `ErrorModel` stránky třídu modelu v *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="a0167-123">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and `ErrorModel` page model class in the *Pages* folder.</span></span>

<span data-ttu-id="a0167-124">V aplikaci MVC nemáte uspořádání metody akce obslužnou rutinu chyby s atributy metody HTTP, jako například `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="a0167-124">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="a0167-125">Explicitní příkazy zabránit v dosažení metodu některých požadavků.</span><span class="sxs-lookup"><span data-stu-id="a0167-125">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="a0167-126">Povolí anonymní přístup k metodě tak, aby byly schopný přijímat zobrazení chyb neověřené uživatele.</span><span class="sxs-lookup"><span data-stu-id="a0167-126">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="a0167-127">Například poskytuje následující metody chyba obslužné rutiny [dotnet nové](/dotnet/core/tools/dotnet-new) šablony MVC a zobrazí se v adaptéru domovské:</span><span class="sxs-lookup"><span data-stu-id="a0167-127">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="a0167-128">Konfigurace stavu znakové stránky</span><span class="sxs-lookup"><span data-stu-id="a0167-128">Configuring status code pages</span></span>

<span data-ttu-id="a0167-129">Ve výchozím nastavení, aplikace neposkytuje znaková stránka bohaté stav pro stavové kódy HTTP, jako například *404 nebyl nalezen*.</span><span class="sxs-lookup"><span data-stu-id="a0167-129">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="a0167-130">Pokud chcete zadat stav znakové stránky, nakonfigurujte Middleware stránky kód stavu přidáním řádek do `Startup.Configure` metoda:</span><span class="sxs-lookup"><span data-stu-id="a0167-130">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="a0167-131">Ve výchozím nastavení přidá Middleware stránky kód stavu jednoduchý, textovém obslužné rutiny pro běžné stavových kódů, třeba 404:</span><span class="sxs-lookup"><span data-stu-id="a0167-131">By default, Status Code Pages Middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![404 stránky](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="a0167-133">Middleware podporuje několik metod rozšíření.</span><span class="sxs-lookup"><span data-stu-id="a0167-133">The middleware supports several extension methods.</span></span> <span data-ttu-id="a0167-134">Jedna metoda má výrazu lambda:</span><span class="sxs-lookup"><span data-stu-id="a0167-134">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="a0167-135">Jiná metoda přebírá obsahu typ a formát řetězce:</span><span class="sxs-lookup"><span data-stu-id="a0167-135">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="a0167-136">Existují také přesměrování a znovu spouštět metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="a0167-136">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="a0167-137">Metoda přesměrování odešle klientovi 302 stavový kód:</span><span class="sxs-lookup"><span data-stu-id="a0167-137">The redirect method sends a 302 status code to the client:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="a0167-138">Znovu spustit metodu vrátí původní kód stavu klienta, ale také provede obslužná rutina pro adresu URL přesměrování:</span><span class="sxs-lookup"><span data-stu-id="a0167-138">The re-execute method returns the original status code to the client but also executes the handler for the redirect URL:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="a0167-139">Stav znakové stránky lze vypnout pro konkrétní požadavky v metodu obslužná rutina stránky Razor nebo kontroler MVC.</span><span class="sxs-lookup"><span data-stu-id="a0167-139">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="a0167-140">Zakázat stav znakové stránky, pokus o načtení [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) v požadavku [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) kolekce a zakažte funkci, pokud je k dispozici:</span><span class="sxs-lookup"><span data-stu-id="a0167-140">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="a0167-141">Pokud se používá `UseStatusCodePages*` přetížení, směřuje na koncový bod v aplikaci, vytvořte zobrazení MVC nebo stránky Razor pro koncový bod.</span><span class="sxs-lookup"><span data-stu-id="a0167-141">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="a0167-142">Například [dotnet nové](/dotnet/core/tools/dotnet-new) šablonu pro stránky Razor aplikace vytváří na následující stránku a třídu modelu stránky:</span><span class="sxs-lookup"><span data-stu-id="a0167-142">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="a0167-143">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a0167-143">*Error.cshtml*:</span></span>

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
    Swapping to <strong>Development</strong> environment will display more detailed information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications</strong>, as it can result in sensitive information from exceptions being displayed to end users. For local debugging, development environment can be enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to <strong>Development</strong>, and restarting the application.
</p>
```

<span data-ttu-id="a0167-144">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a0167-144">*Error.cshtml.cs*:</span></span>

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="a0167-145">Zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="a0167-145">Exception-handling code</span></span>

<span data-ttu-id="a0167-146">Kód v zpracování stránky výjimek můžete vyvolat výjimky.</span><span class="sxs-lookup"><span data-stu-id="a0167-146">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="a0167-147">Často je vhodné pro produkční chybové stránky, které se skládají z čistě statický obsah.</span><span class="sxs-lookup"><span data-stu-id="a0167-147">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="a0167-148">Navíc mějte na paměti, že po odeslání hlaviček pro odpovědi, nelze změnit stavový kód odpovědi na ani můžete všechny výjimky stránky nebo obslužné rutiny spustit.</span><span class="sxs-lookup"><span data-stu-id="a0167-148">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="a0167-149">Odpověď se musí udělat nebo přerušena připojení.</span><span class="sxs-lookup"><span data-stu-id="a0167-149">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="a0167-150">Zpracování výjimek serveru</span><span class="sxs-lookup"><span data-stu-id="a0167-150">Server exception handling</span></span>

<span data-ttu-id="a0167-151">Kromě zpracování logiky aplikace, výjimek [server](xref:fundamentals/servers/index) hostování vaší aplikace provádí některé výjimek.</span><span class="sxs-lookup"><span data-stu-id="a0167-151">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="a0167-152">Pokud server výjimku zachytí před odesláním hlavičky, server odešle *500 – Vnitřní chyba serveru* odpověď se žádný text.</span><span class="sxs-lookup"><span data-stu-id="a0167-152">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="a0167-153">Pokud server výjimku zachytí po odeslání hlaviček, server se ukončí připojení.</span><span class="sxs-lookup"><span data-stu-id="a0167-153">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="a0167-154">Žádosti, které nejsou zpracovávány aplikace jsou zpracovávány serverem.</span><span class="sxs-lookup"><span data-stu-id="a0167-154">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="a0167-155">Jakékoli výjimky jsou zpracována výjimka serveru zpracování.</span><span class="sxs-lookup"><span data-stu-id="a0167-155">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="a0167-156">Žádné nakonfigurovat vlastní chybové stránky nebo middleware výjimek nebo filtry toto chování neovlivňuje.</span><span class="sxs-lookup"><span data-stu-id="a0167-156">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="a0167-157">Spuštění zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="a0167-157">Startup exception handling</span></span>

<span data-ttu-id="a0167-158">Pouze vrstvě hostingu může zpracovávat výjimky, které se uskuteční během spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0167-158">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="a0167-159">Pomocí [webového hostitele](xref:fundamentals/host/web-host), můžete [konfigurace hostitele chování v reakci na chyby během spuštění](xref:fundamentals/host/web-host#detailed-errors) s `captureStartupErrors` a `detailedErrors` klíče.</span><span class="sxs-lookup"><span data-stu-id="a0167-159">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="a0167-160">Hostování můžete jenom zobrazit chybovou stránku pro spuštění zaznamenané chyby, pokud dojde k chybě po adresu nebo port hostitele vazby.</span><span class="sxs-lookup"><span data-stu-id="a0167-160">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="a0167-161">Pokud z nějakého důvodu selže žádné vazby, vrstvě hostování protokoly ke kritické výjimce dotnet procesu havárií, a žádná chybová stránka se zobrazí, když aplikace běží [Kestrel](xref:fundamentals/servers/kestrel) serveru.</span><span class="sxs-lookup"><span data-stu-id="a0167-161">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="a0167-162">Při spuštění [IIS](/iis) nebo [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 selhání procesu* je vrácený [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) Pokud proces nemůže být spustit.</span><span class="sxs-lookup"><span data-stu-id="a0167-162">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="a0167-163">Postupujte podle pomoc při řešení potíží v [řešení potíží s ASP.NET Core ve službě IIS](xref:host-and-deploy/iis/troubleshoot) tématu.</span><span class="sxs-lookup"><span data-stu-id="a0167-163">Follow the troubleshooting advice in the [Troubleshoot ASP.NET Core on IIS](xref:host-and-deploy/iis/troubleshoot) topic.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="a0167-164">Zpracování chyb rozhraní ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="a0167-164">ASP.NET MVC error handling</span></span>

<span data-ttu-id="a0167-165">[MVC](xref:mvc/overview) aplikací mít některé další možnosti pro zpracování chyb, jako je například konfigurace filtry výjimek a provedení ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="a0167-165">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="a0167-166">Filtry výjimek</span><span class="sxs-lookup"><span data-stu-id="a0167-166">Exception Filters</span></span>

<span data-ttu-id="a0167-167">Filtry výjimek lze nastavit globálně nebo na základě-controller nebo na akce v aplikaci MVC.</span><span class="sxs-lookup"><span data-stu-id="a0167-167">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="a0167-168">Tyto filtry zpracovat všechny neošetřené výjimky během provádění akce kontroleru nebo jiný filtr a nejsou s názvem jinak.</span><span class="sxs-lookup"><span data-stu-id="a0167-168">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="a0167-169">Další informace o filtry výjimek v [filtry](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="a0167-169">Learn more about exception filters in [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="a0167-170">Filtry výjimek jsou vhodné pro soutisku výjimky, které se vyskytují v akce MVC, jsou ale není tak účinná jako chyba zpracování middleware.</span><span class="sxs-lookup"><span data-stu-id="a0167-170">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="a0167-171">Dáváte přednost middleware pro obecné případ a použít filtry, pouze pokud potřebujete udělat zpracování chyb *jinak* podle MVC akci, která jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="a0167-171">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="a0167-172">Stav modelu zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="a0167-172">Handling Model State Errors</span></span>

<span data-ttu-id="a0167-173">[Ověření modelu](xref:mvc/models/validation) probíhá před vyvoláním každou akci kontroleru a metoda akce odpovídá kontrola `ModelState.IsValid` a náležitě reagovat.</span><span class="sxs-lookup"><span data-stu-id="a0167-173">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="a0167-174">Některé aplikace se rozhodnete v takovém případě postupujte podle standardní konvence pro řešení chyb při ověřování modelu, [filtru](xref:mvc/controllers/filters) může být příslušné místo pro implementaci tato zásada.</span><span class="sxs-lookup"><span data-stu-id="a0167-174">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="a0167-175">Měli byste otestovat chování vaše akce se stavy neplatný model.</span><span class="sxs-lookup"><span data-stu-id="a0167-175">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="a0167-176">Další informace v [testování řadiče logiku](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="a0167-176">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>
