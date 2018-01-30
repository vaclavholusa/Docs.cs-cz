---
title: "Vysoce výkonné protokolování s LoggerMessage v ASP.NET Core"
author: guardrex
description: "Naučte se používat k vytvoření delegáti lze uložit do mezipaměti, které vyžadují méně objekt přidělení než protokolovacího nástroje rozšiřující metody pro scénáře protokolování vysoce výkonné LoggerMessage funkcí."
manager: wpickett
ms.author: riande
ms.date: 11/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: b155826b5047e88a79d9e339d7bca8885a79006d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="5782f-103">Vysoce výkonné protokolování s LoggerMessage v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5782f-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="5782f-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5782f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5782f-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) funkce vytvořit delegáti lze uložit do mezipaměti, které vyžadují méně přidělování objektů a snižuje nároky na výpočetní výkon než [metody rozšíření protokolovače](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), jako například `LogInformation`, `LogDebug`a `LogError`.</span><span class="sxs-lookup"><span data-stu-id="5782f-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) features create cacheable delegates that require fewer object allocations and reduced computational overhead than [logger extension methods](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), such as `LogInformation`, `LogDebug`, and `LogError`.</span></span> <span data-ttu-id="5782f-106">Pro scénáře protokolování vysoce výkonné, použijte `LoggerMessage` vzor.</span><span class="sxs-lookup"><span data-stu-id="5782f-106">For high-performance logging scenarios, use the `LoggerMessage` pattern.</span></span>

<span data-ttu-id="5782f-107">`LoggerMessage`nabízí následující výhody výkonu přes protokoly rozšiřující metody:</span><span class="sxs-lookup"><span data-stu-id="5782f-107">`LoggerMessage` provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="5782f-108">Metody rozšíření protokolovače vyžadují typy hodnot "zabalení" (převod), jako například `int`, do `object`.</span><span class="sxs-lookup"><span data-stu-id="5782f-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="5782f-109">`LoggerMessage` Vzor zabraňuje zabalení pomocí statické `Action` pole a rozšiřující metody s parametry silného typu.</span><span class="sxs-lookup"><span data-stu-id="5782f-109">The `LoggerMessage` pattern avoids boxing by using static `Action` fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="5782f-110">Metody rozšíření protokolovače musí analyzovat Šablona zprávy (řetězec s názvem formátu) pokaždé, když se zapíše zprávu protokolu.</span><span class="sxs-lookup"><span data-stu-id="5782f-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="5782f-111">`LoggerMessage`vyžaduje pouze jednou analýza šablonu, když je definovaná zpráva.</span><span class="sxs-lookup"><span data-stu-id="5782f-111">`LoggerMessage` only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="5782f-112">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5782f-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5782f-113">Ukázková aplikace ukazuje `LoggerMessage` funkce s základní nabídky systému pro sledování.</span><span class="sxs-lookup"><span data-stu-id="5782f-113">The sample app demonstrates `LoggerMessage` features with a basic quote tracking system.</span></span> <span data-ttu-id="5782f-114">Aplikace přidá a odstraní uvozovek použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="5782f-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="5782f-115">Tyto operace jsou prováděny, zprávy protokolu jsou generovány pomocí `LoggerMessage` vzor.</span><span class="sxs-lookup"><span data-stu-id="5782f-115">As these operations occur, log messages are generated using the `LoggerMessage` pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="5782f-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="5782f-116">LoggerMessage.Define</span></span>

<span data-ttu-id="5782f-117">[Definujte (LogLevel, ID události, řetězec)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) vytvoří `Action` delegovat pro protokolování zprávy.</span><span class="sxs-lookup"><span data-stu-id="5782f-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) creates an `Action` delegate for logging a message.</span></span> <span data-ttu-id="5782f-118">`Define`přetížení povolení předávání až šest parametry typu na řetězec s názvem formátu (šablony).</span><span class="sxs-lookup"><span data-stu-id="5782f-118">`Define` overloads permit passing up to six type parameters to a named format string (template).</span></span>

## <a name="loggermessagedefinescope"></a><span data-ttu-id="5782f-119">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="5782f-119">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="5782f-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) vytvoří `Func` delegovat pro definování [protokolu oboru](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="5782f-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) creates a `Func` delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="5782f-121">`DefineScope`přetížení povolení předávání až tři parametry typu na řetězec s názvem formátu (šablony).</span><span class="sxs-lookup"><span data-stu-id="5782f-121">`DefineScope` overloads permit passing up to three type parameters to a named format string (template).</span></span>

## <a name="message-template-named-format-string"></a><span data-ttu-id="5782f-122">Šablona zprávy (s názvem řetězec formátu)</span><span class="sxs-lookup"><span data-stu-id="5782f-122">Message template (named format string)</span></span>

<span data-ttu-id="5782f-123">Pro zadaný řetězec `Define` a `DefineScope` metody je šablonu a není interpolované řetězce.</span><span class="sxs-lookup"><span data-stu-id="5782f-123">The string provided to the `Define` and `DefineScope` methods is a template and not an interpolated string.</span></span> <span data-ttu-id="5782f-124">Zástupné symboly doplní v pořadí, že jsou uvedené typy.</span><span class="sxs-lookup"><span data-stu-id="5782f-124">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="5782f-125">Zástupné názvy v šabloně by měl být popisný a konzistentní v rámci šablony.</span><span class="sxs-lookup"><span data-stu-id="5782f-125">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="5782f-126">Představují názvy vlastností v rámci strukturovaných dat protokolu.</span><span class="sxs-lookup"><span data-stu-id="5782f-126">They serve as property names within structured log data.</span></span> <span data-ttu-id="5782f-127">Doporučujeme, abyste [Pascal velká a malá písmena](/dotnet/standard/design-guidelines/capitalization-conventions) pro názvy zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="5782f-127">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="5782f-128">Například `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="5782f-128">For example, `{Count}`, `{FirstName}`.</span></span>

## <a name="implementing-loggermessagedefine"></a><span data-ttu-id="5782f-129">Implementace LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="5782f-129">Implementing LoggerMessage.Define</span></span>

<span data-ttu-id="5782f-130">Každé zprávě protokolu je `Action` uchovávat v statické pole vytvořené `LoggerMessage.Define`.</span><span class="sxs-lookup"><span data-stu-id="5782f-130">Each log message is an `Action` held in a static field created by `LoggerMessage.Define`.</span></span> <span data-ttu-id="5782f-131">Například vytvoří ukázkovou aplikaci pole pro popis zprávy protokolu pro požadavek GET na indexovou stránku (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="5782f-131">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="5782f-132">Pro `Action`, zadejte:</span><span class="sxs-lookup"><span data-stu-id="5782f-132">For the `Action`, specify:</span></span>

* <span data-ttu-id="5782f-133">Úroveň protokolování</span><span class="sxs-lookup"><span data-stu-id="5782f-133">The log level.</span></span>
* <span data-ttu-id="5782f-134">Identifikátor události jedinečný ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) s názvem metoda statické rozšíření.</span><span class="sxs-lookup"><span data-stu-id="5782f-134">A unique event identifier ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) with the name of the static extension method.</span></span>
* <span data-ttu-id="5782f-135">Šablona zprávy (s názvem řetězec formátu).</span><span class="sxs-lookup"><span data-stu-id="5782f-135">The message template (named format string).</span></span> 

<span data-ttu-id="5782f-136">Požadavek na indexovou stránku sad ukázkové aplikace:</span><span class="sxs-lookup"><span data-stu-id="5782f-136">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="5782f-137">Úroveň protokolu `Information`.</span><span class="sxs-lookup"><span data-stu-id="5782f-137">Log level to `Information`.</span></span>
* <span data-ttu-id="5782f-138">Id události k `1` s názvem `IndexPageRequested` metoda.</span><span class="sxs-lookup"><span data-stu-id="5782f-138">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="5782f-139">Šablona zprávy (s názvem řetězec formátu) na řetězec.</span><span class="sxs-lookup"><span data-stu-id="5782f-139">Message template (named format string) to a string.</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="5782f-140">Ukládá strukturované protokolování použít název události při se dodává s id události pro obohacení protokolování.</span><span class="sxs-lookup"><span data-stu-id="5782f-140">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="5782f-141">Například [Serilog](https://github.com/serilog/serilog-extensions-logging) používá název události.</span><span class="sxs-lookup"><span data-stu-id="5782f-141">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="5782f-142">`Action` Je volána metodou rozšíření silného typu.</span><span class="sxs-lookup"><span data-stu-id="5782f-142">The `Action` is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="5782f-143">`IndexPageRequested` Metoda přihlásí ukázková aplikace zprávu o požadavek GET Index stránky:</span><span class="sxs-lookup"><span data-stu-id="5782f-143">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="5782f-144">`IndexPageRequested`je volána v protokolovacího nástroje v `OnGetAsync` metoda v *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="5782f-144">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="5782f-145">Zkontrolujte výstup konzoly aplikace:</span><span class="sxs-lookup"><span data-stu-id="5782f-145">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="5782f-146">Chcete-li předat parametry do zprávy protokolu, definujte až šest typů, při vytváření statické pole.</span><span class="sxs-lookup"><span data-stu-id="5782f-146">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="5782f-147">Ukázková aplikace protokoly řetězec při přidávání v uvozovkách definováním `string` zadejte `Action` pole:</span><span class="sxs-lookup"><span data-stu-id="5782f-147">The sample app logs a string when adding a quote by defining a `string` type for the `Action` field:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="5782f-148">Šablona zprávy protokolu delegáta přijímá jeho zástupné hodnoty ze zadané typy.</span><span class="sxs-lookup"><span data-stu-id="5782f-148">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="5782f-149">Ukázková aplikace definuje delegáta pro přidání v uvozovkách, kde parametr uvozovky je `string`:</span><span class="sxs-lookup"><span data-stu-id="5782f-149">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="5782f-150">Metoda statické rozšíření pro přidávání v uvozovkách, `QuoteAdded`, obdrží hodnota argumentu uvozovky a předává jej do `Action` delegovat:</span><span class="sxs-lookup"><span data-stu-id="5782f-150">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the `Action` delegate:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="5782f-151">V modelu stránky indexovou stránku (*Pages/Index.cshtml.cs*), `QuoteAdded` nazývá do protokolu zpráva:</span><span class="sxs-lookup"><span data-stu-id="5782f-151">In the Index page's page model (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="5782f-152">Zkontrolujte výstup konzoly aplikace:</span><span class="sxs-lookup"><span data-stu-id="5782f-152">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="5782f-153">Implementuje aplikace ukázka `try` &ndash; `catch` vzor pro odstranění uvozovky.</span><span class="sxs-lookup"><span data-stu-id="5782f-153">The sample app implements a `try`&ndash;`catch` pattern for quote deletion.</span></span> <span data-ttu-id="5782f-154">Informační zpráva je zaprotokolována pro operace úspěšné odstranění.</span><span class="sxs-lookup"><span data-stu-id="5782f-154">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="5782f-155">Když je vyvolána výjimka, zaprotokoluje se chybová zpráva pro operaci odstranění.</span><span class="sxs-lookup"><span data-stu-id="5782f-155">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="5782f-156">Zprávy protokolu pro neúspěšná operace odstranění zahrnuje trasování zásobníku výjimek (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="5782f-156">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="5782f-157">Všimněte si, jak je předán výjimka delegáta v `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="5782f-157">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="5782f-158">V modelu stránky pro stránku Index odstranění úspěšné uvozovky volá `QuoteDeleted` metodu protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="5782f-158">In the page model for the Index page, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="5782f-159">Když pro odstranění, se nenašel v uvozovkách `ArgumentNullException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="5782f-159">When a quote isn't found for deletion, an `ArgumentNullException` is thrown.</span></span> <span data-ttu-id="5782f-160">Výjimka je zachytí aplikace `try` &ndash; `catch` příkaz a voláním `QuoteDeleteFailed` metodu protokolovacího nástroje v `catch` blok (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5782f-160">The exception is trapped by the `try`&ndash;`catch` statement and logged by calling the `QuoteDeleteFailed` method on the logger in the `catch` block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="5782f-161">Když je v uvozovkách úspěšně odstraněn, zkontrolujte výstup konzoly aplikace:</span><span class="sxs-lookup"><span data-stu-id="5782f-161">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="5782f-162">Pokud se odstranění uvozovky nezdaří, zkontrolujte výstup konzoly aplikace.</span><span class="sxs-lookup"><span data-stu-id="5782f-162">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="5782f-163">Všimněte si, že výjimka je součástí zprávy protokolu:</span><span class="sxs-lookup"><span data-stu-id="5782f-163">Note that the exception is included in the log message:</span></span>

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

## <a name="implementing-loggermessagedefinescope"></a><span data-ttu-id="5782f-164">Implementace LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="5782f-164">Implementing LoggerMessage.DefineScope</span></span>

<span data-ttu-id="5782f-165">Definování [protokolu oboru](xref:fundamentals/logging/index#log-scopes) chcete použít pro řadu zpráv protokolu pomocí [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) metoda.</span><span class="sxs-lookup"><span data-stu-id="5782f-165">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) method.</span></span>

<span data-ttu-id="5782f-166">Ukázková aplikace má **Vymazat vše** tlačítko pro odstranění všechna uvozovky v databázi.</span><span class="sxs-lookup"><span data-stu-id="5782f-166">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="5782f-167">Uvozovky jsou odstraněny je odstranit jednu najednou.</span><span class="sxs-lookup"><span data-stu-id="5782f-167">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="5782f-168">Pokaždé, když je odstraněn v uvozovkách, `QuoteDeleted` metoda je volána v protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="5782f-168">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="5782f-169">Obor protokolu se přidá do těchto zpráv protokolu.</span><span class="sxs-lookup"><span data-stu-id="5782f-169">A log scope is added to these log messages.</span></span>

<span data-ttu-id="5782f-170">Povolit `IncludeScopes` v možnostech protokolovacího nástroje konzoly:</span><span class="sxs-lookup"><span data-stu-id="5782f-170">Enable `IncludeScopes` in the console logger options:</span></span>

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

<span data-ttu-id="5782f-171">Nastavení `IncludeScopes` je nutný v aplikacích ASP.NET 2.0 jádra pro povolení protokolu obory.</span><span class="sxs-lookup"><span data-stu-id="5782f-171">Setting `IncludeScopes` is required in ASP.NET Core 2.0 apps to enable log scopes.</span></span> <span data-ttu-id="5782f-172">Nastavení `IncludeScopes` prostřednictvím *appsettings* konfiguračních souborů je funkce, která má pro verzi ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="5782f-172">Setting `IncludeScopes` via *appsettings* configuration files is a feature that's planned for the ASP.NET Core 2.1 release.</span></span>

<span data-ttu-id="5782f-173">Ukázková aplikace vymaže jiných poskytovatelů a přidá filtry ke snížení výstup protokolování.</span><span class="sxs-lookup"><span data-stu-id="5782f-173">The sample app clears other providers and adds filters to reduce the logging output.</span></span> <span data-ttu-id="5782f-174">Díky tomu je snazší zprávy protokolu ukázka, která ukazují zobrazíte `LoggerMessage` funkce.</span><span class="sxs-lookup"><span data-stu-id="5782f-174">This makes it easier to see the sample's log messages that demonstrate `LoggerMessage` features.</span></span>

<span data-ttu-id="5782f-175">Pokud chcete vytvořit obor protokolu, přidání pole pro uložení `Func` delegovat pro obor.</span><span class="sxs-lookup"><span data-stu-id="5782f-175">To create a log scope, add a field to hold a `Func` delegate for the scope.</span></span> <span data-ttu-id="5782f-176">Ukázková aplikace vytvoří pole s názvem `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="5782f-176">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="5782f-177">Použití `DefineScope` vytvořit delegát.</span><span class="sxs-lookup"><span data-stu-id="5782f-177">Use `DefineScope` to create the delegate.</span></span> <span data-ttu-id="5782f-178">Až tři typy lze zadat pro použití jako argumenty pro šablony při vyvolání delegáta.</span><span class="sxs-lookup"><span data-stu-id="5782f-178">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="5782f-179">Ukázková aplikace používá šablonu zpráva, která obsahuje počet odstraněných nabídek ( `int` typu):</span><span class="sxs-lookup"><span data-stu-id="5782f-179">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="5782f-180">Zadejte metodu statické rozšíření pro zprávy protokolu.</span><span class="sxs-lookup"><span data-stu-id="5782f-180">Provide a static extension method for the log message.</span></span> <span data-ttu-id="5782f-181">Zahrňte všechny parametry typu s názvem vlastnosti, které se zobrazují v šabloně zprávy.</span><span class="sxs-lookup"><span data-stu-id="5782f-181">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="5782f-182">Ukázková aplikace přebírá `count` uvozovky odstranit a vrátí `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="5782f-182">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="5782f-183">Zabalí oboru, volání metod rozšíření protokolování `using` bloku:</span><span class="sxs-lookup"><span data-stu-id="5782f-183">The scope wraps the logging extension calls in a `using` block:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="5782f-184">Zkontrolujte zprávy protokolu v výstup konzoly aplikace.</span><span class="sxs-lookup"><span data-stu-id="5782f-184">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="5782f-185">Následující výsledek ukazuje tři uvozovky odstranit se zprávou protokolu oboru zahrnout:</span><span class="sxs-lookup"><span data-stu-id="5782f-185">The following result shows three quotes deleted with the log scope message included:</span></span>

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

## <a name="see-also"></a><span data-ttu-id="5782f-186">Viz také</span><span class="sxs-lookup"><span data-stu-id="5782f-186">See also</span></span>

* [<span data-ttu-id="5782f-187">Protokolování</span><span class="sxs-lookup"><span data-stu-id="5782f-187">Logging</span></span>](xref:fundamentals/logging/index)
