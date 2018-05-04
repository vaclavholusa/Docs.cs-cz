---
title: Protokolování v ASP.NET Core
author: ardalis
description: Další informace o rozhraní protokolování v ASP.NET Core. Zjistit předdefinované protokolování zprostředkovatele a další informace o oblíbených poskytovatelů třetích stran.
manager: wpickett
ms.author: tdykstra
ms.date: 12/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/index
ms.openlocfilehash: 78dcee05799965c72f878662df61034018a23021
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="logging-in-aspnet-core"></a>Protokolování v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/) a [tní Dykstra](https://github.com/tdykstra)

Jádro ASP.NET podporuje protokolování rozhraní API, která funguje s různými zprostředkovatelů protokolování. Předdefinované zprostředkovatele umožňují odeslat protokoly na jeden nebo více míst, a můžete zařadit rozhraní protokolování třetích stran. Tento článek ukazuje, jak používat rozhraní API pro integrované protokolování a poskytovatelé ve vašem kódu.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([stažení](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="how-to-create-logs"></a>Postup vytvoření protokoly

Pokud chcete vytvořit protokoly, získat `ILogger` objektu z [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru:

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Pak volejte metody protokolování pro tento objekt protokolovacího nástroje:

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Tento příklad vytvoří protokoly s `TodoController` třídy jako *kategorie*. Kategorie jsou vysvětleny [dále v tomto článku](#log-category).

ASP.NET Core neposkytuje asynchronní metody protokolovacího nástroje, protože by měla být tak rychlé, že není vhodné nákladů na použití asynchronních protokolování. Pokud jste v situaci, kde to není pravda, zvažte změnu způsob přihlášení. Pokud data store je pomalé, zápis zpráv protokolu rychlé úložiště nejprve, pak přesuňte je pomalé úložiště později. Například Přihlaste se do fronty zpráv, který má číst a uložit trvale na pomalé úložiště jiným procesem.

## <a name="how-to-add-providers"></a>Postup přidání zprostředkovatelů

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)
Zprostředkovatel protokolování přijímá zprávy, které vytvoříte pomocí `ILogger` objektu a zobrazuje nebo je uloží. Například konzola poskytovatel zprávy zobrazí v konzole a zprostředkovatele služby Azure App Service je uložit do úložiště objektů blob Azure.

Chcete-li použít poskytovatele, volejte poskytovatele `Add<ProviderName>` metoda rozšíření v *Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

Výchozí šablona projektu umožňuje protokolování pomocí [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) metoda:

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Zprostředkovatel protokolování přijímá zprávy, které vytvoříte pomocí `ILogger` objektu a zobrazuje nebo je uloží. Například konzola poskytovatel zprávy zobrazí v konzole a zprostředkovatele služby Azure App Service je uložit do úložiště objektů blob Azure.

Pro použití poskytovatele, nainstalujte jeho balíček NuGet a volání metody rozšíření poskytovatele na instanci `ILoggerFactory`, jak je znázorněno v následujícím příkladu.

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core [vkládání závislostí](xref:fundamentals/dependency-injection) (DI) poskytuje `ILoggerFactory` instance. `AddConsole` a `AddDebug` rozšiřující metody jsou definovány v [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) a [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) balíčky. Každá metoda rozšíření volá `ILoggerFactory.AddProvider` metodu předáním v instanci poskytovatele. 

> [!NOTE]
> Přidá zprostředkovatele protokolování v ukázkové aplikace pro tento článek `Configure` metodu `Startup` – třída. Pokud chcete získat výstup protokolu z kódu, který provede dříve, přidejte zprostředkovatele protokolování v `Startup` místo třídy konstruktor. 

* * *
Najdete zde informace o jednotlivých [integrované protokolování zprostředkovatele](#built-in-logging-providers) a obsahuje odkazy na [poskytovatelů třetích stran protokolování](#third-party-logging-providers) dále v článku.

## <a name="sample-logging-output"></a>Ukázkový výstup protokolování

Ukázkový kód uvedené v předchozí části se zobrazí protokoly v konzole při spuštění z příkazového řádku. Tady je příklad výstupu konzoly:

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```

Tyto protokoly byly vytvořeny tak, že přejdete do `http://localhost:5000/api/todo/0`, která aktivuje spuštění obou `ILogger` volání uvedené v předchozí části.

Tady je příklad stejné protokolů, jak jsou zobrazeny v okně ladění, když spustíte ukázkovou aplikaci v sadě Visual Studio:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

Protokoly, které byly vytvořeny `ILogger` volání uvedené v předchozí části začínat řetězcem "TodoApi.Controllers.TodoController". Protokoly, které začínají kategorie "Microsoft" jsou z ASP.NET Core. ASP.NET Core sám a kódu aplikace používají stejné rozhraní API protokolování a poskytovateli stejné protokolování.

Zbývající část tohoto článku popisuje některé podrobnosti a možnosti pro protokolování.

## <a name="nuget-packages"></a>Balíčky NuGet

`ILogger` a `ILoggerFactory` rozhraní jsou v [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), a výchozí implementace pro ně jsou v [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).

## <a name="log-category"></a>Kategorie protokolu

A *kategorie* je zahrnut v každém protokolu, který vytvoříte. Zadejte kategorii, při vytváření `ILogger` objektu. Kategorie může být libovolný řetězec, ale konvenci, je použít plně kvalifikovaný název třídy, ze kterého se zapisují protokoly. Například: "TodoApi.Controllers.TodoController".

Můžete zadat kategorii jako řetězec nebo použijte metodu rozšíření, která je odvozena kategorii z typu. Chcete-li zadat kategorii jako řetězec, volejte `CreateLogger` na `ILoggerFactory` instance, jak je uvedeno níže.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

Ve většině případů, je jednodušší použít `ILogger<T>`, jako v následujícím příkladu.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Jde o ekvivalent volání `CreateLogger` s názvem plně kvalifikovaný typ `T`.

## <a name="log-level"></a>Úroveň protokolu

Pokaždé, když napíšete protokolu, zadejte jeho [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel). Úroveň protokolu označuje stupeň závažnosti nebo důležitost. Můžete například napsat `Information` protokolu při ukončení metody za normálních okolností `Warning` po návratu metody, návratový kód 404 a protokolu `Error` protokolu při catch neočekávanou výjimku.

V následujícím příkladu kódu, názvy metod (například `LogWarning`) zadejte úroveň protokolu. První parametr je [protokolu událost s ID](#log-event-id). Druhý parametr je [Šablona zprávy](#log-message-template) se zástupnými symboly pro argument hodnoty poskytované zbývající parametry metody. Parametry metody jsou podrobně popsány dále v tomto článku.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Metody protokolu, které obsahují úroveň v název metody jsou [rozšiřující metody pro objektu ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions). Na pozadí volat tyto metody `Log` metody, která přijímá `LogLevel` parametr. Můžete volat `Log` metoda přímo spíše než jeden z těchto metod rozšíření, ale syntaxe je poměrně složitá. Další informace najdete v tématu [objektu ILogger rozhraní](/dotnet/api/microsoft.extensions.logging.ilogger) a [rozšíření protokolovače zdrojový kód](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).

ASP.NET Core definuje následující [protokolu úrovně](/dotnet/api/microsoft.extensions.logging.loglevel), seřazené zde z minimálně na nejvyšší závažnosti.

* Trasování = 0

  Pro informace, které je vhodné pouze pro vývojáře, ladění problém. Tyto zprávy mohou obsahovat citlivé aplikaci data a nemělo by být povolené v produkčním prostředí. *Zakázané ve výchozím nastavení.* Příklad: `Credentials: {"User":"someuser", "Password":"P@ssword"}`

* Ladění = 1

  Informace, která má krátkodobou užitečnost při vývoji a ladění. Příklad: `Entering method Configure with flag set to true.` obvykle nebude povolit `Debug` úroveň protokolů v produkčním prostředí, pokud řešíte, kvůli velkému počtu protokoly.

* Informace o = 2

  Pro sledování obecný tok aplikace. Tyto protokoly obvykle mají některé dlouhodobé hodnoty. Příklad: `Request received for path /api/todo`

* Upozornění = 3

  Nestandardní nebo neočekávané události v toku aplikací. Ty mohou obsahovat chyby nebo jinými podmínkami, které nemáte způsobit zastavení aplikace, ale které může potřebovat nutné prozkoumat. Zpracovávaný výjimky jsou obvyklé místo pro použití `Warning` úrovně protokolování. Příklad: `FileNotFoundException for file quotes.txt.`

* Chyba = 4

  Chyby a výjimky, které nelze zpracovat. Tyto zprávy označují selhání v aktuální aktivita nebo operace (například aktuální požadavek HTTP), není chybu celou aplikaci. Příklad zprávy protokolu: `Cannot insert record due to duplicate key violation.`

* Kritické = 5

  K selhání, které vyžadují okamžitou pozornost. Příklady: ztrátě dat., nedostatek místa na disku.

Úroveň protokolu můžete určit, kolik protokolu výstup zapsán do média konkrétní úložiště a které zobrazí okno. Například v produkčním prostředí můžete všechny protokoly `Information` úroveň a nižší přejít na úložiště dat svazek a všechny protokoly `Warning` úrovně a vyšší přejděte k úložišti dat hodnotu. Během vývoje, může být obvykle odeslat protokoly `Warning` nebo vyšší závažnosti ke konzole. Když potřebujete vyřešit, můžete přidat `Debug` úroveň. [Filtrování protokolu](#log-filtering) později v tomto článku vysvětluje, jak řídit které protokolu úrovně zpracovává zprostředkovatele.

Zapíše rozhraní ASP.NET Core `Debug` protokoly pro framework události na úrovni. Příklady protokolu dříve v tomto článku vyloučené protokoly níže `Information` úroveň, takže žádná `Debug` úrovně protokoly byly vidět. Tady je příklad protokoly konzoly, pokud spustíte ukázkovou aplikaci nakonfigurovat tak, aby zobrazit `Debug` a vyšší protokoly pro zprostředkovatele konzoly.

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a>ID události protokolu

Pokaždé, když napíšete protokolu, můžete zadat *ID události*. Ukázková aplikace k tomu pomocí místně definované `LoggingEvents` třídy:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

ID události je celočíselná hodnota, která můžete přidružit jednu na druhou sadu protokolované události. Například protokolu pro přidání položky do nákupního košíku může být událost ID 1000 a protokolu pro dokončení nákupu může být událost ID 1001.

Ve výstupu protokolování může uložené v poli ID události nebo součástí textové zprávy, v závislosti na zprostředkovateli. Ladění zprostředkovatele nezobrazí ID událostí, ale konzola poskytovatel je zobrazuje v závorkách za kategorií:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Šablona zpráv protokolu

Pokaždé, když napíšete zprávu protokolu poskytnete Šablona zprávy. Šablona zprávy může být řetězec, nebo může obsahovat pojmenované zástupné symboly, do které argument hodnoty budou vloženy. Šablony není řetězec formátu, a zástupné symboly by měla mít název, není číslované.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Pořadí zástupných symbolů, není jejich názvy, určuje, které parametry slouží k zadání jejich hodnot. Pokud máte následující kód:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Výsledný zprávy protokolu vypadá takto:

```
Parameter values: parm1, parm2
```

Rozhraní protokolování zpráv formátování tímto způsobem, aby bylo možné pro protokolování zprostředkovatele implementovat [sémantické protokolování, také známé jako strukturovaný protokolování](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Protože jsou argumenty sami předat protokolování systému, ne jenom šablony formátovaná zpráva zprostředkovatelé protokolování může ukládat hodnoty parametru jako pole kromě Šablona zprávy. Pokud jste odkazovat protokolu výstup Azure Table Storage a volání metody protokolovacího nástroje vypadá takto:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Každá entita Azure Table může mít `ID` a `RequestTime` vlastnosti, které zjednodušuje dotazy na data protokolu. Můžete najít všechny protokoly v rámci konkrétní `RequestTime` rozsah bez nutnosti analyzovat časový limit textové zprávy.

## <a name="logging-exceptions"></a>Protokolování výjimky

Metody protokolovacího nástroje, mají přetížení, které umožňují předat výjimku, jako v následujícím příkladu:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

Různé zprostředkovatele zpracovat informace o výjimce různými způsoby. Tady je příklad výstupu ladění zprostředkovatele z výše uvedeném kódu.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Filtrování protokolu

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)
Můžete zadat úroveň minimální protokolu pro konkrétního zprostředkovatele a kategorie nebo pro všechny poskytovatele nebo všechny kategorie. Žádné protokoly nižší než minimální úroveň nejsou předaný tohoto zprostředkovatele, takže nemusíte získat nezobrazí nebo uložené. 

Pokud chcete potlačit všechny protokoly, můžete zadat `LogLevel.None` jako úroveň minimální protokolu. Celočíselnou hodnotu `LogLevel.None` je 6, která je vyšší než `LogLevel.Critical` (5).

**Vytvoření pravidla filtru v konfiguraci**

Šablony projektů vytvořit kód, který volá `CreateDefaultBuilder` nastavení protokolování pro zprostředkovatele konzoly a ladění. `CreateDefaultBuilder` Metoda také nastaví protokolování tak, aby hledat konfiguraci v `Logging` části pomocí kódu takto:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

Konfigurační data Určuje minimální protokolu úrovně zprostředkovatele a kategorie, jako v následujícím příkladu:

[!code-json[](index/sample2/appsettings.json)]

Tento formát JSON vytvoří šesti pravidla filtru, jeden pro zprostředkovatele ladění, čtyři pro zprostředkovatele konzoly a ten, který se vztahuje na všechny poskytovatele. Zobrazí se později jak právě jeden z těchto pravidel je zvolen pro každého zprostředkovatele při `ILogger` je vytvořen objekt.

**Pravidla filtru v kódu**

V kódu, můžete zaregistrovat pravidla filtru, jak je znázorněno v následujícím příkladu:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

Druhý `AddFilter` Určuje zprostředkovatele, který ladění pomocí jeho názvu typu. První `AddFilter` platí pro všechny poskytovatele, protože neurčuje typ poskytovatele.

**Jak pravidla filtrování se použijí.**

Konfigurační data a `AddFilter` uvedeném v předchozích ukázkách kódu vytvořit pravidla uvedené v následující tabulce. Prvních šest pocházet z příklad konfigurace a poslední dva pocházet z příkladu kódu.

| Číslo | Zprostředkovatel      | Kategorie, které začínají...          | Úroveň minimální protokolu |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | Ladit         | Všechny kategorie                          | Informace o       |
| 2      | Konzola       | Microsoft.AspNetCore.Mvc.Razor.Internal | Upozornění           |
| 3      | Konzola       | Microsoft.AspNetCore.Mvc.Razor.Razor    | Ladit             |
| 4      | Konzola       | Microsoft.AspNetCore.Mvc.Razor          | Chyba             |
| 5      | Konzola       | Všechny kategorie                          | Informace o       |
| 6      | Všichni poskytovatelé | Všechny kategorie                          | Ladit             |
| 7      | Všichni poskytovatelé | Systém                                  | Ladit             |
| 8      | Ladit         | Microsoft                               | trasování             |

Při vytváření `ILogger` objekt zápis protokolů, `ILoggerFactory` objekt vybere jedno pravidlo na zprostředkovatele pro použití tohoto protokolovacího nástroje. Všechny zprávy, které jsou napsané v tomto `ILogger` objekt jsou filtrovány podle vybraná pravidla. Většina konkrétní pravidlo možné pro každou kategorii dvojice a zprostředkovatele se vybere z dostupná pravidla.

Následující algoritmus se používá pro každého zprostředkovatele při `ILogger` se vytvoří pro danou kategorii:

* Vyberte všechna pravidla, které odpovídají zprostředkovatel nebo jeho alias. Pokud nejsou nalezeny, vyberte všechna pravidla s poskytovatele prázdný.
* Od výsledku v předchozím kroku vyberte pravidla s nejdéle odpovídající kategorii předponu. Pokud nejsou nalezeny, vyberte všechna pravidla, které nechcete zadat kategorii.
* Pokud je vybraných víc pravidel trvat **poslední** jeden.
* Pokud je vybrána žádná pravidla, použijte `MinimumLevel`.

Předpokládejme například, máte předchozí seznam pravidel a vytvoříte `ILogger` objektu pro kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":

* Ladění zprostředkovatele platí pravidla 1, 6 a 8. Pravidlo 8 je nejvíce konkrétní tak, aby se jeden vybrané.
* Pro zprostředkovatele konzoly platí pravidla 3, 4, 5 a 6. Pravidlo 3 je nejvíce.

Při vytváření protokoly s `ILogger` pro kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", protokoly z `Trace` úrovni a vyšší přejde na ladění zprostředkovatele a protokoly `Debug` úrovni a vyšší přejde k poskytovateli konzoly.

**Aliasy zprostředkovatele**

Název typu můžete zadat poskytovatele v konfiguraci, ale každý poskytovatel definuje kratší *alias* , je jednodušší použít. Pro předdefinované zprostředkovatele použijte následující aliasy:

- Konzola
- Ladit
- Protokol událostí
- AzureAppServices
- TraceSource
- EventSource

**Výchozí minimální úroveň**

Není k dispozici minimální nastavení úrovně, který se uplatní pouze v případě, že žádná pravidla z konfigurace nebo kód platí pro daného zprostředkovatele a kategorie. Následující příklad ukazuje, jak nastavit minimální úroveň:

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

Pokud není explicitně nastavit minimální úroveň, výchozí hodnota je `Information`, to znamená, že `Trace` a `Debug` protokoly jsou ignorovány.

**Funkce filtru**

Můžete napsat kód ve funkci filtru pro použití pravidel filtrování. Pro všechny zprostředkovatele a kategorie, které nemají přiřazenou konfigurace nebo kód pravidla je volána funkce filtru. Kód ve funkci má přístup k typ zprostředkovatele, kategorie a úroveň protokolu se rozhodnout, zda mají být protokolovány zprávu. Příklad:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Někteří poskytovatelé protokolování umožňují určit, kdy by měla být protokoly na médium úložiště nebo ignorovat na základě úroveň protokolu a kategorie.

`AddConsole` a `AddDebug` metody rozšíření poskytují přetížení, které umožňují předat kritéria filtrování. Následující vzorový kód způsobí, že konzola poskytovatel ignorovat protokoly níže `Warning` úroveň, při ladění zprostředkovatele ignoruje protokoly, které vytvoří rozhraní.

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

`AddEventLog` Metoda má přetížení, které přijímá `EventLogSettings` instanci, která může obsahovat filtrování funkce v jeho `Filter` vlastnost. Zprostředkovatel TraceSource neposkytuje, žádný z těchto přetížení vzhledem k tomu, že jsou na základě jeho úroveň protokolování a dalších parametrů `SourceSwitch` a `TraceListener` používá.

Můžete nastavit pravidla filtrování pro všechny poskytovatele, které jsou registrovány `ILoggerFactory` instance pomocí `WithFilter` metoda rozšíření. Následující příklad omezuje protokoly framework (kategorie začíná "Microsoft" nebo "Systém") a upozornění při upozornění v protokolu aplikace na úrovni ladění.

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Pokud chcete zabránit zápisu pro danou kategorii všechny protokoly pomocí filtrování, můžete zadat `LogLevel.None` jako úroveň minimální protokolu této kategorie. Celočíselnou hodnotu `LogLevel.None` je 6, která je vyšší než `LogLevel.Critical` (5).

`WithFilter` Rozšíření metoda je poskytována [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) balíček NuGet. Metoda vrátí novou `ILoggerFactory` instance, která bude filtrovat zprávy protokolu předaný všechny protokoly poskytovatele registrované s ním. Nemá vliv, jakékoliv `ILoggerFactory` instance, včetně původní `ILoggerFactory` instance.

* * *
## <a name="log-scopes"></a>Obory protokolu

Můžete seskupit sadu logické operace v rámci *oboru* Chcete-li přiřadit každý protokol, který je vytvořen jako součást této sady stejná data. Například můžete každý protokol vytvořené jako součást zpracování transakci zahrnují ID transakce.

Obor je `IDisposable` typ, který je vrácený `ILogger.BeginScope<TState>` metoda a bude trvat, dokud je zrušen. Použít obor podle zabalení vaše protokoly volání metod `using` blokovat, jak je vidět tady:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Následující kód umožňuje obory pro zprostředkovatele konzoly:

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)
V *Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Konfigurace `IncludeScopes` možnost protokolovacího nástroje Konzola je potřeba povolit protokolování obor. Konfigurace `IncludeScopes` pomocí *appsettings* konfigurační soubory bude k dispozici ve verzi ASP.NET Core 2.1.

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
V *Startup.cs*:

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

* * *
Každé zprávě protokolu obsahuje informace o oboru:

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>Zprostředkovatelé integrované protokolování

ASP.NET Core dodává tyto zprostředkovatele:

* [Console](#console)
* [Ladění](#debug)
* [EventSource](#eventsource)
* [EventLog](#eventlog)
* [TraceSource](#tracesource)
* [Aplikační služba Azure](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a>Zprostředkovatel konzoly

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) balíček zprostředkovatele odesílá výstup protokolu ke konzole. 

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)
```csharp
logging.AddConsole()
```

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
```csharp
loggerFactory.AddConsole()
```

[Přetížení AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) umožňují předáte v úroveň minimální protokolu, funkce filtru a logická hodnota, která označuje, zda obory. Další možností je předat `IConfiguration` objektu, který můžete určit podporu obory a úrovně protokolování. 

Pokud uvažujete o poskytovateli konzoly pro použití v produkčním prostředí, mějte na paměti, že má významný dopad na výkon.

Když vytvoříte nový projekt v sadě Visual Studio `AddConsole` metoda vypadá takto:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Tento kód odkazuje `Logging` části *appSettings.JSON určený* souboru:

[!code-json[](index/sample//appsettings.json)]

Nastavení při povolení aplikace zobrazí protokoly framework limit upozornění k protokolování na úrovni ladění, jak je popsáno v [filtrování protokolu](#log-filtering) části. Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).

* * *
<a id="debug"></a>
### <a name="the-debug-provider"></a>Ladění zprostředkovatele

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) balíček zprostředkovatele zapíše výstup protokolu pomocí [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) – třída (`Debug.WriteLine` volání metod).

V systému Linux, tohoto zprostředkovatele zapisuje protokoly do */var/log/message*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

[Přetížení AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) umožňují předáte v úroveň minimální protokolu nebo funkce filtru.

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a>EventSource zprostředkovatele

Pro aplikace, které cílí ASP.NET Core 1.1.0 nebo vyšší, [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) balíček zprostředkovatele můžete implementovat trasování událostí. V systému Windows, používá [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803). Zprostředkovatel je platformy, ale nejsou žádná událost shromažďování a zobrazení nástroje pro Linux nebo systému macOS ještě. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

Dobrým způsobem, jak shromáždit a zobrazit protokoly se má používat [nástroje PerfView nástroj](https://www.microsoft.com/download/details.aspx?id=28567). Existují další nástroje pro prohlížení protokolů trasování událostí pro Windows, ale nástroje PerfView přináší nejlepší výsledky pro práci s události ETW vygenerované pomocí technologie ASP.NET. 

Konfigurace nástroje PerfView pro shromažďování události zapsané podle tohoto zprostředkovatele, přidejte řetězec `*Microsoft-Extensions-Logging` k **další poskytovatele** seznamu. (Nezapomeňte si projít hvězdičky na začátku řetězce.)

![Další poskytovatele nástroje Perfview](index/_static/perfview-additional-providers.png)

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a>Zprostředkovatel protokolu událostí systému Windows

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) balíček zprostředkovatele odesílá výstup protokolu do protokolu událostí systému Windows.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

[Přetížení AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) umožňují předáte v `EventLogSettings` nebo úroveň minimální protokolu.

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a>TraceSource zprostředkovatele

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) používá balíček zprostředkovatele [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) knihovny a zprostředkovatele.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

[Přetížení AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) umožňují předáte v přepínač zdroje a naslouchací proces trasování.

Pro tohoto zprostředkovatele použijte aplikace má ke spuštění na rozhraní .NET Framework (nikoli .NET Core). Umožňuje zprostředkovatele směrování zpráv do různých [naslouchací procesy](/dotnet/framework/debug-trace-profile/trace-listeners), například [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) použít v ukázkové aplikaci.

Následující příklad konfiguruje `TraceSource` zprostředkovatele, který protokoluje `Warning` a vyšší zprávy v okně konzoly.

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a>Zprostředkovatel služby Azure App Service

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) balíček zprostředkovatele zapisuje protokoly do textových souborů v systému souborů aplikace služby Azure App Service a na [úložiště objektů blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) v účtu Azure Storage. Zprostředkovatel je k dispozici pouze pro aplikace, které cílí ASP.NET Core 1.1.0 nebo vyšší. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Pokud cílení na .NET Core, není nutné instalovat balíček zprostředkovatele nebo explicitně volání `AddAzureWebAppDiagnostics`. Zprostředkovatel je automaticky dostupný pro vaši aplikaci při nasazení aplikace do služby Azure App Service.

Pokud cílení na rozhraní .NET Framework, do projektu přidejte balíček zprostředkovatele a vyvolání `AddAzureWebAppDiagnostics`:

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

`AddAzureWebAppDiagnostics` Přetížení vám umožní předat v [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) se kterým můžete přepsat výchozí nastavení, jako je například protokolování výstupu šablony, název objektu blob a omezení velikosti souboru. (*Výstup šablony* je šablona zprávy, který se použije pro všechny protokoly nad ten, který je zadat při volání `ILogger` metoda.)

---

Když nasazujete do aplikace služby App Service, vaše aplikace respektuje nastavení v [diagnostické protokoly](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) části **služby App Service** stránce portálu Azure. Při změně těchto nastavení, změny se projeví okamžitě bez nutnosti restartovat aplikaci nebo znovu nasadit kódu do ní. 

![Nastavení protokolování Azure](index/_static/azure-logging-settings.png)

Výchozí umístění pro soubory protokolu je v *D:\\domácí\\LogFiles\\aplikace* složku a výchozí název souboru je *diagnostiky yyyymmdd.txt*. Výchozí limit velikosti souboru je 10 MB a výchozí maximální počet souborů, které uchovávají se 2. Výchozí název objektu blob je *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*. Další informace o výchozí chování najdete v tématu [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).

Zprostředkovatel funguje pouze při spuštění projektu v prostředí Azure. Nemá žádný vliv, pokud spouštíte místně &mdash; není zapsat do místních souborů nebo vývoj pro místní úložiště pro objekty BLOB.

## <a name="third-party-logging-providers"></a>Zprostředkovatelé třetí strany protokolování

Zde jsou některé rozhraní protokolování třetích stran, které pracují s ASP.NET Core:

* [elmah.IO](https://github.com/elmahio/Elmah.Io.Extensions.Logging) -zprostředkovatele pro službu Elmah.Io

* [JSNLog](http://jsnlog.com) -protokoly výjimek jazyka JavaScript a dalších událostí na straně klienta pomocí protokolu na straně serveru.

* [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) -zprostředkovatele pro službu Loggr

* [NLog](https://github.com/NLog/NLog.Extensions.Logging) -zprostředkovatele pro knihovnu NLog

* [Serilog](https://github.com/serilog/serilog-extensions-logging) -zprostředkovatele pro knihovnu Serilog

Můžete provést některé architektury třetích stran [sémantické protokolování, také známé jako strukturovaný protokolování](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Pomocí rozhraní třetích stran je podobná pomocí jedné z předdefinované zprostředkovatele: Přidejte balíček NuGet do projektu a volání metody rozšíření na `ILoggerFactory`. Další informace najdete v dokumentaci k každý framework.

## <a name="azure-log-streaming"></a>Streamování protokolů Azure

Vysílání datového proudu protokolů Azure umožňuje zobrazit aktivitu protokolu v reálném čase z: 

* Aplikační server 
* Webový server
* Trasování neúspěšných žádostí 

Postup konfigurace streamování protokolů Azure: 

* Přejděte na **protokolů diagnostiky** stránky ze stránky portálu vaší aplikace
* Nastavit **protokolování aplikací (systém souborů)** na on. 

![Stránka Azure portálu diagnostických protokolů](index/_static/azure-diagnostic-logs.png)

Přejděte na **vysílání datového proudu protokolu** stránky zobrazení zpráv aplikace. Jste přihlášení pomocí aplikace prostřednictvím `ILogger` rozhraní. 

![Vysílání datového proudu protokolu aplikace portálu Azure](index/_static/azure-log-streaming.png)


## <a name="see-also"></a>Viz také

[Vysoce výkonné protokolování s LoggerMessage](xref:fundamentals/logging/loggermessage)
