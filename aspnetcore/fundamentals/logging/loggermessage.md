---
title: "Vysoce výkonné protokolování s LoggerMessage v ASP.NET Core"
author: guardrex
description: "Naučte se používat k vytvoření delegáti lze uložit do mezipaměti, které vyžadují méně objekt přidělení než protokolovacího nástroje rozšiřující metody pro scénáře protokolování vysoce výkonné LoggerMessage funkcí."
ms.author: riande
manager: wpickett
ms.date: 11/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: defba75c6c9ea13d24af4cd8515d82d9e7cf9853
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a>Vysoce výkonné protokolování s LoggerMessage v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) funkce vytvořit delegáti lze uložit do mezipaměti, které vyžadují méně přidělování objektů a snižuje nároky na výpočetní výkon než [metody rozšíření protokolovače](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), jako například `LogInformation`, `LogDebug`a `LogError`. Pro scénáře protokolování vysoce výkonné, použijte `LoggerMessage` vzor.

`LoggerMessage`nabízí následující výhody výkonu přes protokoly rozšiřující metody:

* Metody rozšíření protokolovače vyžadují typy hodnot "zabalení" (převod), jako například `int`, do `object`. `LoggerMessage` Vzor zabraňuje zabalení pomocí statické `Action` pole a rozšiřující metody s parametry silného typu.
* Metody rozšíření protokolovače musí analyzovat Šablona zprávy (řetězec s názvem formátu) pokaždé, když se zapíše zprávu protokolu. `LoggerMessage`vyžaduje pouze jednou analýza šablonu, když je definovaná zpráva.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Ukázková aplikace ukazuje `LoggerMessage` funkce s základní nabídky systému pro sledování. Aplikace přidá a odstraní uvozovek použití databáze v paměti. Tyto operace jsou prováděny, zprávy protokolu jsou generovány pomocí `LoggerMessage` vzor.

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[Definujte (LogLevel, ID události, řetězec)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) vytvoří `Action` delegovat pro protokolování zprávy. `Define`přetížení povolení předávání až šest parametry typu na řetězec s názvem formátu (šablony).

## <a name="loggermessagedefinescope"></a>LoggerMessage.DefineScope

[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) vytvoří `Func` delegovat pro definování [protokolu oboru](xref:fundamentals/logging/index#log-scopes). `DefineScope`přetížení povolení předávání až tři parametry typu na řetězec s názvem formátu (šablony).

## <a name="message-template-named-format-string"></a>Šablona zprávy (s názvem řetězec formátu)

Pro zadaný řetězec `Define` a `DefineScope` metody je šablonu a není interpolované řetězce. Zástupné symboly doplní v pořadí, že jsou uvedené typy. Zástupné názvy v šabloně by měl být popisný a konzistentní v rámci šablony. Představují názvy vlastností v rámci strukturovaných dat protokolu. Doporučujeme, abyste [Pascal velká a malá písmena](/dotnet/standard/design-guidelines/capitalization-conventions) pro názvy zástupný symbol. Například `{Count}`, `{FirstName}`.

## <a name="implementing-loggermessagedefine"></a>Implementace LoggerMessage.Define

Každé zprávě protokolu je `Action` uchovávat v statické pole vytvořené `LoggerMessage.Define`. Například vytvoří ukázkovou aplikaci pole pro popis zprávy protokolu pro požadavek GET na indexovou stránku (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

Pro `Action`, zadejte:

* Úroveň protokolování
* Identifikátor události jedinečný ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) s názvem metoda statické rozšíření.
* Šablona zprávy (s názvem řetězec formátu). 

Požadavek na indexovou stránku sad ukázkové aplikace:

* Úroveň protokolu `Information`.
* Id události k `1` s názvem `IndexPageRequested` metoda.
* Šablona zprávy (s názvem řetězec formátu) na řetězec.

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

Ukládá strukturované protokolování použít název události při se dodává s id události pro obohacení protokolování. Například [Serilog](https://github.com/serilog/serilog-extensions-logging) používá název události.

`Action` Je volána metodou rozšíření silného typu. `IndexPageRequested` Metoda přihlásí ukázková aplikace zprávu o požadavek GET Index stránky:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested`je volána v protokolovacího nástroje v `OnGetAsync` metoda v *Pages/Index.cshtml.cs*:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

Zkontrolujte výstup konzoly aplikace:

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

Chcete-li předat parametry do zprávy protokolu, definujte až šest typů, při vytváření statické pole. Ukázková aplikace protokoly řetězec při přidávání v uvozovkách definováním `string` zadejte `Action` pole:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

Šablona zprávy protokolu delegáta přijímá jeho zástupné hodnoty ze zadané typy. Ukázková aplikace definuje delegáta pro přidání v uvozovkách, kde parametr uvozovky je `string`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

Metoda statické rozšíření pro přidávání v uvozovkách, `QuoteAdded`, obdrží hodnota argumentu uvozovky a předává jej do `Action` delegovat:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

V souboru kódu na pozadí indexovou stránku (*Pages/Index.cshtml.cs*), `QuoteAdded` nazývá do protokolu zpráva:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

Zkontrolujte výstup konzoly aplikace:

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

Implementuje aplikace ukázka `try` &ndash; `catch` vzor pro odstranění uvozovky. Informační zpráva je zaprotokolována pro operace úspěšné odstranění. Když je vyvolána výjimka, zaprotokoluje se chybová zpráva pro operaci odstranění. Zprávy protokolu pro neúspěšná operace odstranění zahrnuje trasování zásobníku výjimek (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

Všimněte si, jak je předán výjimka delegáta v `QuoteDeleteFailed`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

V modelu Index stránky code-behind, odstranění úspěšné uvozovky volá `QuoteDeleted` metodu protokolovacího nástroje. Když pro odstranění, se nenašel v uvozovkách `ArgumentNullException` je vyvolána výjimka. Výjimka je zachytí aplikace `try` &ndash; `catch` příkaz a voláním `QuoteDeleteFailed` metodu protokolovacího nástroje v `catch` blok (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

Když je v uvozovkách úspěšně odstraněn, zkontrolujte výstup konzoly aplikace:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

Pokud se odstranění uvozovky nezdaří, zkontrolujte výstup konzoly aplikace. Všimněte si, že výjimka je součástí zprávy protokolu:

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

## <a name="implementing-loggermessagedefinescope"></a>Implementace LoggerMessage.DefineScope

Definování [protokolu oboru](xref:fundamentals/logging/index#log-scopes) chcete použít pro řadu zpráv protokolu pomocí [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) metoda.

Ukázková aplikace má **Vymazat vše** tlačítko pro odstranění všechna uvozovky v databázi. Uvozovky jsou odstraněny je odstranit jednu najednou. Pokaždé, když je odstraněn v uvozovkách, `QuoteDeleted` metoda je volána v protokolovacího nástroje. Obor protokolu se přidá do těchto zpráv protokolu.

Povolit `IncludeScopes` v možnostech protokolovacího nástroje konzoly:

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

Nastavení `IncludeScopes` je nutný v aplikacích ASP.NET 2.0 jádra pro povolení protokolu obory. Nastavení `IncludeScopes` prostřednictvím *appsettings* konfiguračních souborů je funkce, která má pro verzi ASP.NET Core 2.1.

Ukázková aplikace vymaže jiných poskytovatelů a přidá filtry ke snížení výstup protokolování. Díky tomu je snazší zprávy protokolu ukázka, která ukazují zobrazíte `LoggerMessage` funkce.

Pokud chcete vytvořit obor protokolu, přidání pole pro uložení `Func` delegovat pro obor. Ukázková aplikace vytvoří pole s názvem `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

Použití `DefineScope` vytvořit delegát. Až tři typy lze zadat pro použití jako argumenty pro šablony při vyvolání delegáta. Ukázková aplikace používá šablonu zpráva, která obsahuje počet odstraněných nabídek ( `int` typu):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

Zadejte metodu statické rozšíření pro zprávy protokolu. Zahrňte všechny parametry typu s názvem vlastnosti, které se zobrazují v šabloně zprávy. Ukázková aplikace přebírá `count` uvozovky odstranit a vrátí `_allQuotesDeletedScope`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

Zabalí oboru, volání metod rozšíření protokolování `using` bloku:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

Zkontrolujte zprávy protokolu v výstup konzoly aplikace. Následující výsledek ukazuje tři uvozovky odstranit se zprávou protokolu oboru zahrnout:

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

## <a name="see-also"></a>Viz také

* [Protokolování](xref:fundamentals/logging/index)
