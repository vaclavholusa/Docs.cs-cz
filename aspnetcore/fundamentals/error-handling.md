---
title: "Zpracování chyb v ASP.NET Core"
author: ardalis
description: "Můžete zjistit, jak se budou zpracovávat chyby v aplikacích ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: cab395645d46c56a1a89464a8e8e716a296a9637
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="11327-103">Úvod do zpracování chyb v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="11327-103">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="11327-104">Podle [Steve Smith](https://ardalis.com/) a [tní Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="11327-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="11327-105">Tento článek popisuje běžné appoaches pro zpracování chyb v aplikacích ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="11327-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="11327-106">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="11327-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="11327-107">Stránka výjimka vývojáře</span><span class="sxs-lookup"><span data-stu-id="11327-107">The developer exception page</span></span>

<span data-ttu-id="11327-108">Konfigurace aplikace pro zobrazení stránky, které jsou uvedeny podrobné informace o výjimkách, nainstalujte `Microsoft.AspNetCore.Diagnostics` NuGet balíček a přidat čáru, která [konfigurace metody ve třídě spuštění](startup.md):</span><span class="sxs-lookup"><span data-stu-id="11327-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="11327-109">Uveďte `UseDeveloperExceptionPage` před veškerý middleware chcete zachytit výjimky, jako například `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="11327-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="11327-110">Povolit výjimky stránce vývojáře **jenom když aplikace běží ve vývojovém prostředí**.</span><span class="sxs-lookup"><span data-stu-id="11327-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="11327-111">Nechcete sdílet informace podrobné výjimky veřejně při spuštění aplikace v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="11327-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="11327-112">[Další informace o konfiguraci prostředí](environments.md).</span><span class="sxs-lookup"><span data-stu-id="11327-112">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="11327-113">Pokud chcete zobrazit stránku výjimka vývojáře, spusťte ukázkovou aplikaci s prostředím nastavena na `Development`a přidejte `?throw=true` pro základní adresu URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="11327-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="11327-114">Stránka obsahuje několik karet s informacemi o výjimku a požadavek.</span><span class="sxs-lookup"><span data-stu-id="11327-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="11327-115">Na první kartě zahrnuje trasování zásobníku.</span><span class="sxs-lookup"><span data-stu-id="11327-115">The first tab includes a stack trace.</span></span> 

![Trasování zásobníku](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="11327-117">Další karta ukazuje dotaz parametrů řetězce, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="11327-117">The next tab shows the query string parameters, if any.</span></span>

![Parametrů řetězce dotazu](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="11327-119">Tento požadavek nebyly k dispozici žádné soubory cookie, ale pokud neodpovídala, by se na **soubory cookie** kartě. Můžete zobrazit seznam hlaviček, které byly předány na kartě poslední.</span><span class="sxs-lookup"><span data-stu-id="11327-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Záhlaví](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="11327-121">Konfigurace vlastní výjimky zpracování stránky</span><span class="sxs-lookup"><span data-stu-id="11327-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="11327-122">Je vhodné nakonfigurovat stránku obslužná rutina výjimky pro použití při není aplikace spuštěna `Development` prostředí.</span><span class="sxs-lookup"><span data-stu-id="11327-122">It's a good idea to configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="11327-123">V aplikaci MVC nemáte uspořádání explicitně metody akce obslužnou rutinu chyby s atributy metody HTTP, jako například `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="11327-123">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="11327-124">Použití explicitní příkazů může zabránit dosažení metodu některých požadavků.</span><span class="sxs-lookup"><span data-stu-id="11327-124">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="11327-125">Konfigurace stavu znakové stránky</span><span class="sxs-lookup"><span data-stu-id="11327-125">Configuring status code pages</span></span>

<span data-ttu-id="11327-126">Ve výchozím nastavení nebudou aplikace zadejte znaková stránka bohaté stav stavové kódy HTTP, jako je například 500 (vnitřní chyba serveru) nebo 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="11327-126">By default, your app won't provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="11327-127">Můžete nakonfigurovat `StatusCodePagesMiddleware` přidáním řádek do `Configure` metoda:</span><span class="sxs-lookup"><span data-stu-id="11327-127">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="11327-128">Ve výchozím nastavení přidá tento middleware jednoduchý, textovém obslužné rutiny pro běžné stavových kódů, třeba 404:</span><span class="sxs-lookup"><span data-stu-id="11327-128">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![404 stránky](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="11327-130">Middleware podporuje několik různých rozšiřující metody.</span><span class="sxs-lookup"><span data-stu-id="11327-130">The middleware supports several different extension methods.</span></span> <span data-ttu-id="11327-131">Má výrazu lambda, jiné přijímá řetězec typ a formát obsahu.</span><span class="sxs-lookup"><span data-stu-id="11327-131">One takes a lambda expression, another takes a content type and format string.</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="11327-132">Existují také metody rozšíření přesměrování.</span><span class="sxs-lookup"><span data-stu-id="11327-132">There are also redirect extension methods.</span></span> <span data-ttu-id="11327-133">Jeden odešle klientovi 302 stavový kód a jeden vrátí původní stavový kód pro klienta, ale také provede obslužná rutina pro adresa URL pro přesměrování.</span><span class="sxs-lookup"><span data-stu-id="11327-133">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="11327-134">Pokud je nutné zakázat stav znakové stránky pro určité požadavky, můžete to udělat:</span><span class="sxs-lookup"><span data-stu-id="11327-134">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="11327-135">Zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="11327-135">Exception-handling code</span></span>

<span data-ttu-id="11327-136">Kód v zpracování stránky výjimek můžete vyvolat výjimky.</span><span class="sxs-lookup"><span data-stu-id="11327-136">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="11327-137">Často je vhodné pro produkční chybové stránky, které se skládají z čistě statický obsah.</span><span class="sxs-lookup"><span data-stu-id="11327-137">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="11327-138">Navíc mějte na paměti, že po odeslání hlaviček pro odpovědi, nelze změnit stavový kód odpovědi na ani můžete všechny výjimky stránky nebo obslužné rutiny spustit.</span><span class="sxs-lookup"><span data-stu-id="11327-138">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="11327-139">Odpověď se musí udělat nebo přerušena připojení.</span><span class="sxs-lookup"><span data-stu-id="11327-139">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="11327-140">Zpracování výjimek serveru</span><span class="sxs-lookup"><span data-stu-id="11327-140">Server exception handling</span></span>

<span data-ttu-id="11327-141">Kromě zpracování logiky aplikace, výjimek [server](servers/index.md) hostování vaší aplikace provádí některé výjimek.</span><span class="sxs-lookup"><span data-stu-id="11327-141">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app performs some exception handling.</span></span> <span data-ttu-id="11327-142">Pokud server výjimku zachytí před odesláním hlavičky, server odešle 500 Vnitřní chyba serveru odpověď se žádný text.</span><span class="sxs-lookup"><span data-stu-id="11327-142">If the server catches an exception before the headers are sent, the server sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="11327-143">Pokud server výjimku zachytí po odeslání hlaviček, server se ukončí připojení.</span><span class="sxs-lookup"><span data-stu-id="11327-143">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="11327-144">Žádosti, které nejsou zpracovávány aplikace jsou zpracovávány serverem.</span><span class="sxs-lookup"><span data-stu-id="11327-144">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="11327-145">Jakékoli výjimky jsou zpracována výjimka serveru zpracování.</span><span class="sxs-lookup"><span data-stu-id="11327-145">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="11327-146">Žádné nakonfigurovat vlastní chybové stránky nebo middleware výjimek nebo filtry toto chování neovlivňuje.</span><span class="sxs-lookup"><span data-stu-id="11327-146">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="11327-147">Spuštění zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="11327-147">Startup exception handling</span></span>

<span data-ttu-id="11327-148">Pouze vrstvě hostingu může zpracovávat výjimky, které se uskuteční během spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="11327-148">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="11327-149">Můžete [konfigurace hostitele chování v reakci na chyby během spuštění](hosting.md#detailed-errors) pomocí `captureStartupErrors` a `detailedErrors` klíč.</span><span class="sxs-lookup"><span data-stu-id="11327-149">You can [configure how the host behaves in response to errors during startup](hosting.md#detailed-errors) using `captureStartupErrors` and the `detailedErrors` key.</span></span>

<span data-ttu-id="11327-150">Hostování můžete jenom zobrazit chybovou stránku pro spuštění zaznamenané chyby, pokud dojde k chybě po adresu nebo port hostitele vazby.</span><span class="sxs-lookup"><span data-stu-id="11327-150">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="11327-151">Pokud z nějakého důvodu selže žádné vazby, vrstvě hostování protokoly ke kritické výjimce dotnet procesu havárií, a žádná chybová stránka se zobrazí, když aplikace běží [Kestrel](xref:fundamentals/servers/kestrel) serveru.</span><span class="sxs-lookup"><span data-stu-id="11327-151">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="11327-152">Při spuštění [IIS](/iis) nebo [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 selhání procesu* je vrácený [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) Pokud proces nemůže být spustit.</span><span class="sxs-lookup"><span data-stu-id="11327-152">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="11327-153">Postupujte podle pomoc při řešení potíží v [řešení potíží s ASP.NET Core ve službě IIS](xref:host-and-deploy/iis/troubleshoot) tématu.</span><span class="sxs-lookup"><span data-stu-id="11327-153">Follow the troubleshooting advice in the [Troubleshoot ASP.NET Core on IIS](xref:host-and-deploy/iis/troubleshoot) topic.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="11327-154">Zpracování chyb rozhraní ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="11327-154">ASP.NET MVC error handling</span></span>

<span data-ttu-id="11327-155">[MVC](xref:mvc/overview) aplikací mít některé další možnosti pro zpracování chyb, jako je například konfigurace filtry výjimek a provedení ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="11327-155">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="11327-156">Filtry výjimek</span><span class="sxs-lookup"><span data-stu-id="11327-156">Exception Filters</span></span>

<span data-ttu-id="11327-157">Filtry výjimek lze nastavit globálně nebo na základě-controller nebo na akce v aplikaci MVC.</span><span class="sxs-lookup"><span data-stu-id="11327-157">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="11327-158">Tyto filtry zpracovat všechny neošetřené výjimky během provádění akce kontroleru nebo jiný filtr a nejsou s názvem jinak.</span><span class="sxs-lookup"><span data-stu-id="11327-158">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="11327-159">Další informace o filtry výjimek v [filtry](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="11327-159">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="11327-160">Filtry výjimek jsou vhodné pro soutisku výjimky, které se vyskytují v akce MVC, jsou ale není tak účinná jako chyba zpracování middleware.</span><span class="sxs-lookup"><span data-stu-id="11327-160">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="11327-161">Dáváte přednost middleware pro obecné případ a použít filtry, pouze pokud potřebujete udělat zpracování chyb *jinak* podle MVC akci, která jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="11327-161">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="11327-162">Stav modelu zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="11327-162">Handling Model State Errors</span></span>

<span data-ttu-id="11327-163">[Ověření modelu](../mvc/models/validation.md) probíhá před vyvoláním každou akci kontroleru a metoda akce odpovídá kontrola `ModelState.IsValid` a náležitě reagovat.</span><span class="sxs-lookup"><span data-stu-id="11327-163">[Model validation](../mvc/models/validation.md) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="11327-164">Některé aplikace se rozhodnete v takovém případě postupujte podle standardní konvence pro řešení chyb při ověřování modelu, [filtru](../mvc/controllers/filters.md) může být příslušné místo pro implementaci tato zásada.</span><span class="sxs-lookup"><span data-stu-id="11327-164">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="11327-165">Měli byste otestovat chování vaše akce se stavy neplatný model.</span><span class="sxs-lookup"><span data-stu-id="11327-165">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="11327-166">Další informace v [testování řadiče logiku](../mvc/controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="11327-166">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



