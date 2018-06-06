---
title: Vysoce výkonné protokolování s LoggerMessage v ASP.NET Core
author: guardrex
description: Další informace o použití LoggerMessage vytvořit Uložitelný delegáti, které vyžadují méně objekt přidělení pro scénáře protokolování vysoce výkonné.
manager: wpickett
ms.author: riande
ms.date: 11/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: 5b5bd03b6cb5da693f046653a09ba400ee6ff585
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729191"
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="7dc7b-103">Vysoce výkonné protokolování s LoggerMessage v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7dc7b-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="7dc7b-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7dc7b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7dc7b-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) funkce vytvořit Uložitelný delegáti, které vyžadují méně přidělování objektů a menší režijní náklady na výpočetní ve srovnání s [metody rozšíření protokolovače](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), jako například `LogInformation`, `LogDebug`, a `LogError`.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) features create cacheable delegates that require fewer object allocations and reduced computational overhead compared to [logger extension methods](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), such as `LogInformation`, `LogDebug`, and `LogError`.</span></span> <span data-ttu-id="7dc7b-106">Pro scénáře protokolování vysoce výkonné, použijte `LoggerMessage` vzor.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-106">For high-performance logging scenarios, use the `LoggerMessage` pattern.</span></span>

<span data-ttu-id="7dc7b-107">`LoggerMessage` nabízí následující výhody výkonu přes protokoly rozšiřující metody:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-107">`LoggerMessage` provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="7dc7b-108">Metody rozšíření protokolovače vyžadují typy hodnot "zabalení" (převod), jako například `int`, do `object`.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="7dc7b-109">`LoggerMessage` Vzor zabraňuje zabalení pomocí statické `Action` pole a rozšiřující metody s parametry silného typu.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-109">The `LoggerMessage` pattern avoids boxing by using static `Action` fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="7dc7b-110">Metody rozšíření protokolovače musí analyzovat Šablona zprávy (řetězec s názvem formátu) pokaždé, když se zapíše zprávu protokolu.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="7dc7b-111">`LoggerMessage` vyžaduje pouze jednou analýza šablonu, když je definovaná zpráva.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-111">`LoggerMessage` only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="7dc7b-112">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7dc7b-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7dc7b-113">Ukázková aplikace ukazuje `LoggerMessage` funkce s základní nabídky systému pro sledování.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-113">The sample app demonstrates `LoggerMessage` features with a basic quote tracking system.</span></span> <span data-ttu-id="7dc7b-114">Aplikace přidá a odstraní uvozovek použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="7dc7b-115">Tyto operace jsou prováděny, zprávy protokolu jsou generovány pomocí `LoggerMessage` vzor.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-115">As these operations occur, log messages are generated using the `LoggerMessage` pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="7dc7b-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="7dc7b-116">LoggerMessage.Define</span></span>

<span data-ttu-id="7dc7b-117">[Definujte (LogLevel, ID události, řetězec)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) vytvoří `Action` delegovat pro protokolování zprávy.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) creates an `Action` delegate for logging a message.</span></span> <span data-ttu-id="7dc7b-118">`Define` přetížení povolení předávání až šest parametry typu na řetězec s názvem formátu (šablony).</span><span class="sxs-lookup"><span data-stu-id="7dc7b-118">`Define` overloads permit passing up to six type parameters to a named format string (template).</span></span>

<span data-ttu-id="7dc7b-119">Pro zadaný řetězec `Define` je metoda šablonu a není interpolované řetězce.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-119">The string provided to the `Define` method is a template and not an interpolated string.</span></span> <span data-ttu-id="7dc7b-120">Zástupné symboly doplní v pořadí, že jsou uvedené typy.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-120">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="7dc7b-121">Zástupné názvy v šabloně by měl být popisný a konzistentní v rámci šablony.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-121">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="7dc7b-122">Představují názvy vlastností v rámci strukturovaných dat protokolu.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-122">They serve as property names within structured log data.</span></span> <span data-ttu-id="7dc7b-123">Doporučujeme, abyste [Pascal velká a malá písmena](/dotnet/standard/design-guidelines/capitalization-conventions) pro názvy zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-123">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="7dc7b-124">Například `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-124">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="7dc7b-125">Každé zprávě protokolu je `Action` uchovávat v statické pole vytvořené `LoggerMessage.Define`.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-125">Each log message is an `Action` held in a static field created by `LoggerMessage.Define`.</span></span> <span data-ttu-id="7dc7b-126">Například vytvoří ukázkovou aplikaci pole pro popis zprávy protokolu pro požadavek GET na indexovou stránku (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="7dc7b-126">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="7dc7b-127">Pro `Action`, zadejte:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-127">For the `Action`, specify:</span></span>

* <span data-ttu-id="7dc7b-128">Úroveň protokolování</span><span class="sxs-lookup"><span data-stu-id="7dc7b-128">The log level.</span></span>
* <span data-ttu-id="7dc7b-129">Identifikátor události jedinečný ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) s názvem metoda statické rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-129">A unique event identifier ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) with the name of the static extension method.</span></span>
* <span data-ttu-id="7dc7b-130">Šablona zprávy (s názvem řetězec formátu).</span><span class="sxs-lookup"><span data-stu-id="7dc7b-130">The message template (named format string).</span></span> 

<span data-ttu-id="7dc7b-131">Požadavek na indexovou stránku sad ukázkové aplikace:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-131">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="7dc7b-132">Úroveň protokolu `Information`.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-132">Log level to `Information`.</span></span>
* <span data-ttu-id="7dc7b-133">Id události k `1` s názvem `IndexPageRequested` metoda.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-133">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="7dc7b-134">Šablona zprávy (s názvem řetězec formátu) na řetězec.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-134">Message template (named format string) to a string.</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="7dc7b-135">Ukládá strukturované protokolování použít název události při se dodává s id události pro obohacení protokolování.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-135">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="7dc7b-136">Například [Serilog](https://github.com/serilog/serilog-extensions-logging) používá název události.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-136">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="7dc7b-137">`Action` Je volána metodou rozšíření silného typu.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-137">The `Action` is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="7dc7b-138">`IndexPageRequested` Metoda přihlásí ukázková aplikace zprávu o požadavek GET Index stránky:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-138">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="7dc7b-139">`IndexPageRequested` je volána v protokolovacího nástroje v `OnGetAsync` metoda v *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-139">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="7dc7b-140">Zkontrolujte výstup konzoly aplikace:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-140">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="7dc7b-141">Chcete-li předat parametry do zprávy protokolu, definujte až šest typů, při vytváření statické pole.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-141">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="7dc7b-142">Ukázková aplikace protokoly řetězec při přidávání v uvozovkách definováním `string` zadejte `Action` pole:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-142">The sample app logs a string when adding a quote by defining a `string` type for the `Action` field:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="7dc7b-143">Šablona zprávy protokolu delegáta přijímá jeho zástupné hodnoty ze zadané typy.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-143">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="7dc7b-144">Ukázková aplikace definuje delegáta pro přidání v uvozovkách, kde parametr uvozovky je `string`:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-144">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="7dc7b-145">Metoda statické rozšíření pro přidávání v uvozovkách, `QuoteAdded`, obdrží hodnota argumentu uvozovky a předává jej do `Action` delegovat:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-145">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the `Action` delegate:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="7dc7b-146">V modelu stránky indexovou stránku (*Pages/Index.cshtml.cs*), `QuoteAdded` nazývá do protokolu zpráva:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-146">In the Index page's page model (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="7dc7b-147">Zkontrolujte výstup konzoly aplikace:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-147">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="7dc7b-148">Implementuje aplikace ukázka `try` &ndash; `catch` vzor pro odstranění uvozovky.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-148">The sample app implements a `try`&ndash;`catch` pattern for quote deletion.</span></span> <span data-ttu-id="7dc7b-149">Informační zpráva je zaprotokolována pro operace úspěšné odstranění.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-149">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="7dc7b-150">Když je vyvolána výjimka, zaprotokoluje se chybová zpráva pro operaci odstranění.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-150">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="7dc7b-151">Zprávy protokolu pro neúspěšná operace odstranění zahrnuje trasování zásobníku výjimek (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="7dc7b-151">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="7dc7b-152">Všimněte si, jak je předán výjimka delegáta v `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-152">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="7dc7b-153">V modelu stránky pro stránku Index odstranění úspěšné uvozovky volá `QuoteDeleted` metodu protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-153">In the page model for the Index page, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="7dc7b-154">Když pro odstranění, se nenašel v uvozovkách `ArgumentNullException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-154">When a quote isn't found for deletion, an `ArgumentNullException` is thrown.</span></span> <span data-ttu-id="7dc7b-155">Výjimka je zachytí aplikace `try` &ndash; `catch` příkaz a voláním `QuoteDeleteFailed` metodu protokolovacího nástroje v `catch` blok (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="7dc7b-155">The exception is trapped by the `try`&ndash;`catch` statement and logged by calling the `QuoteDeleteFailed` method on the logger in the `catch` block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="7dc7b-156">Když je v uvozovkách úspěšně odstraněn, zkontrolujte výstup konzoly aplikace:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-156">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="7dc7b-157">Pokud se odstranění uvozovky nezdaří, zkontrolujte výstup konzoly aplikace.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-157">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="7dc7b-158">Všimněte si, že výjimka je součástí zprávy protokolu:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-158">Note that the exception is included in the log message:</span></span>

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T](T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() in 
      <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="loggermessagedefinescope"></a><span data-ttu-id="7dc7b-159">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="7dc7b-159">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="7dc7b-160">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) vytvoří `Func` delegovat pro definování [protokolu oboru](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="7dc7b-160">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) creates a `Func` delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="7dc7b-161">`DefineScope` přetížení povolení předávání až tři parametry typu na řetězec s názvem formátu (šablony).</span><span class="sxs-lookup"><span data-stu-id="7dc7b-161">`DefineScope` overloads permit passing up to three type parameters to a named format string (template).</span></span>

<span data-ttu-id="7dc7b-162">Stejně jako případě `Define` metoda, pro zadaný řetězec `DefineScope` je metoda šablonu a není interpolované řetězce.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-162">As is the case with the `Define` method, the string provided to the `DefineScope` method is a template and not an interpolated string.</span></span> <span data-ttu-id="7dc7b-163">Zástupné symboly doplní v pořadí, že jsou uvedené typy.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-163">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="7dc7b-164">Zástupné názvy v šabloně by měl být popisný a konzistentní v rámci šablony.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-164">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="7dc7b-165">Představují názvy vlastností v rámci strukturovaných dat protokolu.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-165">They serve as property names within structured log data.</span></span> <span data-ttu-id="7dc7b-166">Doporučujeme, abyste [Pascal velká a malá písmena](/dotnet/standard/design-guidelines/capitalization-conventions) pro názvy zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-166">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="7dc7b-167">Například `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-167">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="7dc7b-168">Definování [protokolu oboru](xref:fundamentals/logging/index#log-scopes) chcete použít pro řadu zpráv protokolu pomocí [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) metoda.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-168">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) method.</span></span>

<span data-ttu-id="7dc7b-169">Ukázková aplikace má **Vymazat vše** tlačítko pro odstranění všechna uvozovky v databázi.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-169">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="7dc7b-170">Uvozovky jsou odstraněny je odstranit jednu najednou.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-170">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="7dc7b-171">Pokaždé, když je odstraněn v uvozovkách, `QuoteDeleted` metoda je volána v protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-171">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="7dc7b-172">Obor protokolu se přidá do těchto zpráv protokolu.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-172">A log scope is added to these log messages.</span></span>

<span data-ttu-id="7dc7b-173">Povolit `IncludeScopes` v části protokoly konzoly *appSettings.JSON určený*:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-173">Enable `IncludeScopes` in the console logger section of *appsettings.json*:</span></span>

[!code-csharp[](loggermessage/sample/appsettings.json?highlight=3-5)]

<span data-ttu-id="7dc7b-174">Pokud chcete vytvořit obor protokolu, přidání pole pro uložení `Func` delegovat pro obor.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-174">To create a log scope, add a field to hold a `Func` delegate for the scope.</span></span> <span data-ttu-id="7dc7b-175">Ukázková aplikace vytvoří pole s názvem `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="7dc7b-175">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="7dc7b-176">Použití `DefineScope` vytvořit delegát.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-176">Use `DefineScope` to create the delegate.</span></span> <span data-ttu-id="7dc7b-177">Až tři typy lze zadat pro použití jako argumenty pro šablony při vyvolání delegáta.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-177">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="7dc7b-178">Ukázková aplikace používá šablonu zpráva, která obsahuje počet odstraněných nabídek ( `int` typu):</span><span class="sxs-lookup"><span data-stu-id="7dc7b-178">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="7dc7b-179">Zadejte metodu statické rozšíření pro zprávy protokolu.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-179">Provide a static extension method for the log message.</span></span> <span data-ttu-id="7dc7b-180">Zahrňte všechny parametry typu s názvem vlastnosti, které se zobrazují v šabloně zprávy.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-180">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="7dc7b-181">Ukázková aplikace přebírá `count` uvozovky odstranit a vrátí `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-181">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="7dc7b-182">Zabalí oboru, volání metod rozšíření protokolování `using` bloku:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-182">The scope wraps the logging extension calls in a `using` block:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="7dc7b-183">Zkontrolujte zprávy protokolu v výstup konzoly aplikace.</span><span class="sxs-lookup"><span data-stu-id="7dc7b-183">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="7dc7b-184">Následující výsledek ukazuje tři uvozovky odstranit se zprávou protokolu oboru zahrnout:</span><span class="sxs-lookup"><span data-stu-id="7dc7b-184">The following result shows three quotes deleted with the log scope message included:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

## <a name="additional-resources"></a><span data-ttu-id="7dc7b-185">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7dc7b-185">Additional resources</span></span>

* [<span data-ttu-id="7dc7b-186">Protokolování</span><span class="sxs-lookup"><span data-stu-id="7dc7b-186">Logging</span></span>](xref:fundamentals/logging/index)
