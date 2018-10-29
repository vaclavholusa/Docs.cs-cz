---
title: Vysoce výkonné protokolování pomocí LoggerMessage v ASP.NET Core
author: guardrex
description: Další informace o použití LoggerMessage k vytváření delegátů možné ukládat do mezipaměti, které vyžadují méně přidělení objektů pro scénáře protokolování vysoce výkonné.
ms.author: riande
ms.date: 11/03/2017
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: a0080a20fed2d8fc295e55822c11d5731c6910ca
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207508"
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a>Vysoce výkonné protokolování pomocí LoggerMessage v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) funkcí vytvořit možné ukládat do mezipaměti delegáty, které vyžadují méně přidělení objektů a snížené nadměrnou výpočetní zátěž ve srovnání s [metody rozšíření protokolování](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), jako například `LogInformation`, `LogDebug`, a `LogError`. Pro scénáře protokolování vysoce výkonné, použijte `LoggerMessage` vzor.

`LoggerMessage` poskytuje následující výhody výkonu prostřednictvím metody rozšíření protokolování:

* Metody rozšíření protokolování vyžadují "zabalení" (převod) typy hodnot, jako například `int`, do `object`. `LoggerMessage` Vzor se vyhnete zabalení pomocí statické `Action` pole a metody rozšíření se silnými typy parametrů.
* Metody rozšíření protokolování musí parsovat šablonu zprávy (pojmenované formátovací řetězec) pokaždé, když je zapsat zprávu protokolu. `LoggerMessage` vyžaduje pouze pokud zpráva je definován jednou analýze šablony.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([stažení](xref:index#how-to-download-a-sample))

Ukázková aplikace předvádí `LoggerMessage` funkce s nabídkou Základní sledování systému. Aplikace přidá a odstraní uvozovky použití databáze v paměti. Tyto operace jsou prováděny, zprávy protokolu jsou generovány pomocí `LoggerMessage` vzor.

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[Definování (LogLevel, ID události, řetězce)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) vytvoří `Action` delegování pro protokolování zprávy. `Define` přetížení povolit předávání až šest parametry typu řetězec s názvem formátu (šablona).

Řetězec k dispozici na `Define` metoda je šablony a ne interpolovaného řetězce. Zástupné symboly jsou vyplněny v pořadí, že jsou uvedeny typy. Zástupné názvy v šabloně by měl být popisný a konzistentní vzhledem k aplikacím v rámci šablony. Slouží jako názvy vlastností v rámci strukturovaná data protokolu. Doporučujeme [Pascal malých a velkých písmen](/dotnet/standard/design-guidelines/capitalization-conventions) pro zástupné názvy. Například `{Count}`, `{FirstName}`.

Jednotlivé zprávy protokolu `Action` uchovávat v datovém typu statické pole vytvořeného touto `LoggerMessage.Define`. Například ukázkové aplikace vytvoří pole pro popis zprávy protokolu pro požadavek GET na indexovou stránku (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

Pro `Action`, zadejte:

* Úroveň protokolování
* Události jedinečný identifikátor ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) s názvem metody statické rozšíření.
* Šablona zprávy (s názvem formátovací řetězec). 

Žádost o indexovou stránku ukázkových aplikací sad:

* Úroveň protokolování `Information`.
* Id události k `1` názvem `IndexPageRequested` metody.
* Šablona zprávy (s názvem formátovací řetězec) na řetězec.

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

Strukturované protokolování úložiště použít název události při se dodává s id události rozšiřuje protokolování. Například [Serilog](https://github.com/serilog/serilog-extensions-logging) používá název události.

`Action` Vyvolat prostřednictvím metody rozšíření silného typu. `IndexPageRequested` Metoda zaznamená zprávu pro požadavek GET Index stránky v ukázkové aplikaci:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested` je volána v protokolovací nástroj v `OnGetAsync` metoda *Pages/Index.cshtml.cs*:

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

Zkontrolujte výstup na konzole aplikace:

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

Pro předání parametrů do zprávy protokolu, definujte až šest typů, při vytváření statické pole. Ukázková aplikace zaznamenává řetězec při přidávání nabídky definováním `string` zadejte `Action` pole:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

Šablona zprávy protokolu delegáta přijímá jeho zástupné hodnoty z typů, které jsou k dispozici. Ukázková aplikace definuje delegáta pro přidání poptávku, kde parametr nabídky `string`:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

Statické rozšíření metoda pro přidání nabídky `QuoteAdded`, přijímá nabídku hodnotu argumentu a předává jej do `Action` delegáta:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

V modelu stránky indexovou stránku (*Pages/Index.cshtml.cs*), `QuoteAdded` je volána k protokolování zprávy:

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

Zkontrolujte výstup na konzole aplikace:

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

Implementuje ukázkové aplikace `try` &ndash; `catch` vzor pro odstranění nabídky. Informační zpráva je zaprotokolována pro úspěšné operaci. Operace odstranění je zaznamenána chybová zpráva, když dojde k výjimce. Zpráva protokolu pro neúspěšné operace odstranění obsahuje trasování zásobníku výjimek (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

Všimněte si, jak je výjimka předaný delegátovi v `QuoteDeleteFailed`:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

V modelu stránky pro indexovou stránku odstranění úspěšné nabídky volá `QuoteDeleted` metodu na protokolovacího nástroje. Pokud není nalezen nabídky pro odstranění, `ArgumentNullException` je vyvolána výjimka. Výjimka je zachycena ve `try` &ndash; `catch` příkazu nebude úspěšné a voláním `QuoteDeleteFailed` metodu na protokolovací nástroj v `catch` blok (*Pages/Index.cshtml.cs*):

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

Pokud je nabídka byla úspěšně odstraněna, zkontrolujte výstup na konzole aplikace:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

Při odstranění nabídky nezdaří, zkontrolujte, zda výstup na konzole aplikace. Všimněte si, že výjimka je součástí zprávy protokolu:

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

## <a name="loggermessagedefinescope"></a>LoggerMessage.DefineScope

[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) vytvoří `Func` delegování pro definování [protokolu oboru](xref:fundamentals/logging/index#log-scopes). `DefineScope` přetížení povolit předávání až tři parametry typu řetězec s názvem formátu (šablona).

Stejně jako v případě s `Define` metoda, řetězec k dispozici na `DefineScope` metoda je šablony a ne interpolovaného řetězce. Zástupné symboly jsou vyplněny v pořadí, že jsou uvedeny typy. Zástupné názvy v šabloně by měl být popisný a konzistentní vzhledem k aplikacím v rámci šablony. Slouží jako názvy vlastností v rámci strukturovaná data protokolu. Doporučujeme [Pascal malých a velkých písmen](/dotnet/standard/design-guidelines/capitalization-conventions) pro zástupné názvy. Například `{Count}`, `{FirstName}`.

Definovat [protokolu oboru](xref:fundamentals/logging/index#log-scopes) použít řadu zpráv protokolu pomocí [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) metoda.

Ukázková aplikace má **Vymazat vše** tlačítko pro odstranění všech nabídek v databázi. Uvozovky budou odstraněny tak, že odeberete jeden po druhém. Pokaždé, když se odstraní uvozovky, `QuoteDeleted` metoda je volána na protokolovacího nástroje. Rozsah protokolu je přidán do těchto zpráv protokolu.

Povolit `IncludeScopes` v části protokolovací nástroj konzoly *appsettings.json*:

[!code-csharp[](loggermessage/sample/appsettings.json?highlight=3-5)]

Vytvoření oboru protokolu, přidáte pole pro uložení `Func` delegování pro obor. Ukázková aplikace vytvoří pole s názvem `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

Použití `DefineScope` k vytvoření delegáta. Až tři typy lze používat jako argumenty šablony při vyvolání delegáta. Ukázková aplikace používá šablonu zprávy, která obsahuje počet odstraněných nabídky ( `int` typ):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

Poskytuje metodu statické rozšíření pro zprávy protokolu. Zahrňte všechny parametry typu pro pojmenované vlastnosti, které se zobrazují v šablonu zprávy. Ukázková aplikace přijímá `count` uvozovkami, aby byly odstranit a vrátí `_allQuotesDeletedScope`:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

Obor zabalí volání rozšíření protokolování `using` blok:

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

Kontrola protokolu zpráv ve výstupu konzoly aplikace. Následující výsledek ukazuje tři uvozovky odstranit obor zprávou protokolu zahrnout:

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

## <a name="additional-resources"></a>Další zdroje

* [Protokolování](xref:fundamentals/logging/index)
