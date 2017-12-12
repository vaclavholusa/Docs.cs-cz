---
title: "Zpracování chyb v ASP.NET Core"
author: ardalis
description: "Můžete zjistit, jak se budou zpracovávat chyby v aplikacích ASP.NET Core."
keywords: "ASP.NET Core, zpracování, chyb zpracování výjimek"
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.assetid: 4db51023-c8a6-4119-bbbe-3917e272c260
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: de2ba0ff9ad17c198c06b510ecfb49f808721bdf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="1ff1e-104">Úvod do zpracování chyb v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ff1e-104">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="1ff1e-105">Podle [Steve Smith](https://ardalis.com/) a [tní Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="1ff1e-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="1ff1e-106">Tento článek popisuje běžné appoaches pro zpracování chyb v aplikacích ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-106">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="1ff1e-107">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1ff1e-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="1ff1e-108">Stránka výjimka vývojáře</span><span class="sxs-lookup"><span data-stu-id="1ff1e-108">The developer exception page</span></span>

<span data-ttu-id="1ff1e-109">Konfigurace aplikace pro zobrazení stránky, které jsou uvedeny podrobné informace o výjimkách, nainstalujte `Microsoft.AspNetCore.Diagnostics` NuGet balíček a přidat čáru, která [konfigurace metody ve třídě spuštění](startup.md):</span><span class="sxs-lookup"><span data-stu-id="1ff1e-109">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="1ff1e-110">Uveďte `UseDeveloperExceptionPage` před veškerý middleware chcete zachytit výjimky, jako například `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-110">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="1ff1e-111">Povolit výjimky stránce vývojáře **jenom když aplikace běží ve vývojovém prostředí**.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-111">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="1ff1e-112">Nechcete sdílet informace podrobné výjimky veřejně při spuštění aplikace v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-112">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="1ff1e-113">[Další informace o konfiguraci prostředí](environments.md).</span><span class="sxs-lookup"><span data-stu-id="1ff1e-113">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="1ff1e-114">Pokud chcete zobrazit stránku výjimka vývojáře, spusťte ukázkovou aplikaci s prostředím nastavena na `Development`a přidejte `?throw=true` pro základní adresu URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-114">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="1ff1e-115">Stránka obsahuje několik karet s informacemi o výjimku a požadavek.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-115">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="1ff1e-116">Na první kartě zahrnuje trasování zásobníku.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-116">The first tab includes a stack trace.</span></span> 

![Trasování zásobníku](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="1ff1e-118">Další karta ukazuje dotaz parametrů řetězce, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-118">The next tab shows the query string parameters, if any.</span></span>

![Parametrů řetězce dotazu](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="1ff1e-120">Tento požadavek nebyly k dispozici žádné soubory cookie, ale pokud neodpovídala, by se na **soubory cookie** kartě. Můžete zobrazit seznam hlaviček, které byly předány na kartě poslední.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-120">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Záhlaví](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="1ff1e-122">Konfigurace vlastní výjimky zpracování stránky</span><span class="sxs-lookup"><span data-stu-id="1ff1e-122">Configuring a custom exception handling page</span></span>

<span data-ttu-id="1ff1e-123">Je vhodné nakonfigurovat stránku obslužná rutina výjimky pro použití při není aplikace spuštěna `Development` prostředí.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-123">It's a good idea to configure an exception handler page to use when the app is not running in the `Development` environment.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="1ff1e-124">V aplikaci MVC nemáte uspořádání explicitně metody akce obslužnou rutinu chyby s atributy metody HTTP, jako například `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-124">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="1ff1e-125">Použití explicitní příkazů může zabránit dosažení metodu některých požadavků.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-125">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="1ff1e-126">Konfigurace stavu znakové stránky</span><span class="sxs-lookup"><span data-stu-id="1ff1e-126">Configuring status code pages</span></span>

<span data-ttu-id="1ff1e-127">Ve výchozím nastavení nebudou aplikace zadejte znaková stránka bohaté stav stavové kódy HTTP, jako je například 500 (vnitřní chyba serveru) nebo 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="1ff1e-127">By default, your app will not provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="1ff1e-128">Můžete nakonfigurovat `StatusCodePagesMiddleware` přidáním řádek do `Configure` metoda:</span><span class="sxs-lookup"><span data-stu-id="1ff1e-128">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="1ff1e-129">Ve výchozím nastavení přidá tento middleware jednoduchý, textovém obslužné rutiny pro běžné stavových kódů, třeba 404:</span><span class="sxs-lookup"><span data-stu-id="1ff1e-129">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![404 stránky](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="1ff1e-131">Middleware podporuje několik různých rozšiřující metody.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-131">The middleware supports several different extension methods.</span></span> <span data-ttu-id="1ff1e-132">Má výrazu lambda, jiné přijímá řetězec typ a formát obsahu.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-132">One takes a lambda expression, another takes a content type and format string.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="1ff1e-133">Existují také metody rozšíření přesměrování.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-133">There are also redirect extension methods.</span></span> <span data-ttu-id="1ff1e-134">Jeden odešle klientovi 302 stavový kód a jeden vrátí původní stavový kód pro klienta, ale také provede obslužná rutina pro adresa URL pro přesměrování.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-134">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="1ff1e-135">Pokud je nutné zakázat stav znakové stránky pro určité požadavky, můžete to udělat:</span><span class="sxs-lookup"><span data-stu-id="1ff1e-135">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="1ff1e-136">Zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="1ff1e-136">Exception-handling code</span></span>

<span data-ttu-id="1ff1e-137">Kód v zpracování stránky výjimek můžete vyvolat výjimky.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-137">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="1ff1e-138">Často je vhodné pro produkční chybové stránky, které se skládají z čistě statický obsah.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-138">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="1ff1e-139">Navíc mějte na paměti, že po odeslání hlaviček pro odpovědi, nelze změnit stavový kód odpovědi na ani můžete všechny výjimky stránky nebo obslužné rutiny spustit.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-139">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="1ff1e-140">Odpověď se musí udělat nebo přerušena připojení.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-140">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="1ff1e-141">Zpracování výjimek serveru</span><span class="sxs-lookup"><span data-stu-id="1ff1e-141">Server exception handling</span></span>

<span data-ttu-id="1ff1e-142">Kromě zpracování logiky aplikace, výjimek [server](servers/index.md) hostování vaší aplikace provádí některé výjimek.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-142">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app performs some exception handling.</span></span> <span data-ttu-id="1ff1e-143">Pokud server výjimku zachytí před odesláním hlavičky, server odešle 500 Vnitřní chyba serveru odpověď se žádný text.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-143">If the server catches an exception before the headers are sent, the server sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="1ff1e-144">Pokud server výjimku zachytí po odeslání hlaviček, server se ukončí připojení.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-144">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="1ff1e-145">Žádosti, které nejsou zpracovávány aplikace jsou zpracovávány serverem.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-145">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="1ff1e-146">Jakékoli výjimky jsou zpracována výjimka serveru zpracování.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-146">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="1ff1e-147">Žádné nakonfigurovat vlastní chybové stránky nebo middleware výjimek nebo filtry toto chování neovlivňuje.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-147">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="1ff1e-148">Spuštění zpracování výjimek</span><span class="sxs-lookup"><span data-stu-id="1ff1e-148">Startup exception handling</span></span>

<span data-ttu-id="1ff1e-149">Pouze vrstvě hostingu může zpracovávat výjimky, které se uskuteční během spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-149">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="1ff1e-150">Můžete [konfigurace hostitele chování v reakci na chyby během spuštění](hosting.md#detailed-errors) pomocí `captureStartupErrors` a `detailedErrors` klíč.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-150">You can [configure how the host behaves in response to errors during startup](hosting.md#detailed-errors) using `captureStartupErrors` and the `detailedErrors` key.</span></span>

<span data-ttu-id="1ff1e-151">Hostování můžete jenom zobrazit chybovou stránku pro spuštění zaznamenané chyby, pokud dojde k chybě po adresu nebo port hostitele vazby.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-151">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="1ff1e-152">Pokud z nějakého důvodu selže žádné vazby, vrstvě hostování zaprotokoluje kritické výjimky, dotnet procesu havárií, a zobrazí se stránka žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-152">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="1ff1e-153">Zpracování chyb rozhraní ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="1ff1e-153">ASP.NET MVC error handling</span></span>

<span data-ttu-id="1ff1e-154">[MVC](../mvc/index.md) aplikací mít některé další možnosti pro zpracování chyb, jako je například konfigurace filtry výjimek a provedení ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-154">[MVC](../mvc/index.md) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="1ff1e-155">Filtry výjimek</span><span class="sxs-lookup"><span data-stu-id="1ff1e-155">Exception Filters</span></span>

<span data-ttu-id="1ff1e-156">Filtry výjimek lze nastavit globálně nebo na základě-controller nebo na akce v aplikaci MVC.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-156">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="1ff1e-157">Tyto filtry zpracovat všechny neošetřené výjimky během provádění akce kontroleru nebo jiný filtr a nejsou s názvem jinak.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-157">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="1ff1e-158">Další informace o filtry výjimek v [filtry](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="1ff1e-158">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="1ff1e-159">Filtry výjimek jsou vhodné pro soutisku výjimky, které se vyskytují v akce MVC, jsou ale není tak účinná jako chyba zpracování middleware.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-159">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="1ff1e-160">Dáváte přednost middleware pro obecné případ a použít filtry, pouze pokud potřebujete udělat zpracování chyb *jinak* podle MVC akci, která jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-160">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="1ff1e-161">Stav modelu zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="1ff1e-161">Handling Model State Errors</span></span>

<span data-ttu-id="1ff1e-162">[Ověření modelu](../mvc/models/validation.md) dojde před každou volaná akce kontroleru a metoda akce odpovídá kontrola `ModelState.IsValid` a náležitě reagovat.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-162">[Model validation](../mvc/models/validation.md) occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="1ff1e-163">Některé aplikace se rozhodnete v takovém případě postupujte podle standardní konvence pro řešení chyb při ověřování modelu, [filtru](../mvc/controllers/filters.md) může být příslušné místo pro implementaci tato zásada.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-163">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="1ff1e-164">Měli byste otestovat chování vaše akce se stavy neplatný model.</span><span class="sxs-lookup"><span data-stu-id="1ff1e-164">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="1ff1e-165">Další informace v [testování řadiče logiku](../mvc/controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="1ff1e-165">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



