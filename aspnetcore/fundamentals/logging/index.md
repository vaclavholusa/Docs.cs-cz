---
title: Protokolování v ASP.NET Core
author: ardalis
description: Další informace o protokolovacího rozhraní v ASP.NET Core. Objevte poskytovatelé vestavěné protokolování a další informace o Oblíbené zprostředkovatele třetí strany.
ms.author: tdykstra
ms.date: 12/15/2017
uid: fundamentals/logging/index
ms.openlocfilehash: dde01129bb7ea29544c4c416dfe9b5522a738d01
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938482"
---
# <a name="logging-in-aspnet-core"></a>Protokolování v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/) a [Petr Dykstra](https://github.com/tdykstra)

ASP.NET Core podporuje protokolování rozhraní API, která funguje s různých poskytovatelů protokolování. Předdefinované zprostředkovatele umožní odeslat protokoly do jednoho nebo více cílů a je možné připojit v rámci protokolování třetích stran. Tento článek ukazuje, jak použít rozhraní API pro vestavěné protokolování a poskytovatelé ve vašem kódu.

::: moniker range=">= aspnetcore-2.0"

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([stažení](xref:tutorials/index#how-to-download-a-sample))

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

::: moniker-end

Informace o protokolování stdout při hostování za nástrojem službou IIS najdete v tématu <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>. Informace týkající se stdout protokolování pomocí služby Azure App Service najdete v tématu <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.

## <a name="how-to-create-logs"></a>Jak vytvořit protokoly

Chcete-li vytvořit protokoly, implementovat [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) objektu z [injektáž závislostí](xref:fundamentals/dependency-injection) kontejneru:

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

K tomuto objektu protokolovač zavoláním metody protokolování:

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Tento příklad vytvoří protokolů pomocí `TodoController` třídy jako *kategorie*. Kategorie jsou vysvětleny [dále v tomto článku](#log-category).

ASP.NET Core neposkytuje asynchronní metody protokolování, protože protokolování by měl být tak rychle, že není vhodné náklady na použití modifikátoru async. Pokud jste v situaci, kde, který není true, zvažte možnost změnit způsob, jakým jste přihlášení. Pokud vaše úložiště dat je pomalá, nejprve zápis zpráv protokolu do rychlého úložiště a pak přesuňte je pomalé úložiště později. Protokolujte například, do fronty zpráv, která má číst a trvale uložena do úložiště pomalé jiným procesem.

## <a name="how-to-add-providers"></a>Přidání poskytovatele

::: moniker range=">= aspnetcore-2.0"

Protokolování zprostředkovatele přijímá zprávy, které vytvoříte pomocí `ILogger` objekt zobrazí a uloží je. Například konzola poskytovatel zobrazí zprávy na konzole a zprostředkovatele služby Azure App Service můžete ukládat ve službě Azure blob storage.

Chcete-li použít poskytovatele, zavolejte poskytovatele `Add<ProviderName>` metody rozšíření v *Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

Povolí protokolování pomocí výchozí šablony projektu [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) metody:

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Protokolování zprostředkovatele přijímá zprávy, které vytvoříte pomocí `ILogger` objekt zobrazí a uloží je. Například konzola poskytovatel zobrazí zprávy na konzole a zprostředkovatele služby Azure App Service můžete ukládat ve službě Azure blob storage.

Používat poskytovatele, nainstalujte svůj balíček NuGet a volání metody rozšíření zprostředkovatele na instanci [implementaci třídy ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), jak je znázorněno v následujícím příkladu:

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core [injektáž závislostí](xref:fundamentals/dependency-injection) (DI) poskytuje `ILoggerFactory` instance. `AddConsole` a `AddDebug` metody rozšíření jsou definovány v [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) a [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) balíčky. Každá metoda rozšíření volá `ILoggerFactory.AddProvider` metodu instance zprostředkovatele. 

> [!NOTE]
> [Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) přidá poskytovatele protokolování v `Startup.Configure` metody. Pokud chcete získat výstup protokolu z kódu, který se spustí dříve, přidejte zprostředkovatele protokolování v `Startup` konstruktoru třídy.

::: moniker-end

Zjistíte informace o jednotlivých [vestavěné protokolování zprostředkovatele](#built-in-logging-providers) a obsahuje odkazy na [zprostředkovatele přihlášení třetí strany](#third-party-logging-providers) dále v tomto článku.

## <a name="settings-file-configuration"></a>Konfigurace nastavení souboru

Každý z předchozích příkladů v [Přidání poskytovatelů](#how-to-add-providers) načte zprostředkovatele konfigurace protokolování z část `Logging` části souborů s nastavením aplikace. Následující příklad ukazuje obsah typické *appsettings. Development.JSON* souboru:

::: moniker range=">= aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": "true"
    }
  }
}
```

`LogLevel` klíče představují názvy protokolů. `Default` Klíč platí do protokolů, které nejsou výslovně uvedena. Hodnota představuje [úrovně protokolování](#log-level) použitý pro daný protokol. Protokol klíče této sady `IncludeScopes` (`Console` v příkladu), zadejte, pokud [protokolu obory](#log-scopes) jsou povolené pro zadaný protokol.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

`LogLevel` klíče představují názvy protokolů. `Default` Klíč platí do protokolů, které nejsou výslovně uvedena. Hodnota představuje [úrovně protokolování](#log-level) použitý pro daný protokol.

::: moniker-end

## <a name="sample-logging-output"></a>Ukázkový výstup protokolování

Ukázkový kód je znázorněno v předchozí části zobrazí protokoly konzoly při spuštění z příkazového řádku. Tady je příklad výstupu konzoly:

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

Tyto protokoly, které byly vytvořeny tak, že přejdete do `http://localhost:5000/api/todo/0`, která aktivuje spuštění obou `ILogger` volání je znázorněno v předchozí části.

Tady je příklad stejného protokolů, jak se objeví v okně ladění, když spustíte ukázkovou aplikaci v sadě Visual Studio:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

Protokoly, které byly vytvořeny `ILogger` volání je znázorněno v předchozí části začínají řetězcem "TodoApi.Controllers.TodoController". Protokoly, které začínají řetězcem "Microsoft". kategorie jsou z ASP.NET Core. ASP.NET Core, samotného a kódu aplikace používají stejné protokolování rozhraní API a stejní poskytovatelé protokolování.

Zbývající část tohoto článku popisuje některé podrobnosti a možností pro protokolování.

## <a name="nuget-packages"></a>Balíčky NuGet

`ILogger` a `ILoggerFactory` rozhraní jsou v [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), a mají výchozí implementace pro ně [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).

## <a name="log-category"></a>Kategorie protokolu

A *kategorie* je součástí každý protokol, který vytvoříte. Zadejte kategorii při vytváření `ILogger` objektu. Kategorie může být libovolný řetězec, ale konvence, je použít plně kvalifikovaný název třídy, ze kterého se zapisují protokoly. Příklad: "TodoApi.Controllers.TodoController".

Můžete určit kategorii jako řetězec nebo používat metodu rozšíření, který se odvozuje od typu kategorie. Chcete-li zadat kategorii, kterou jako řetězec, zavolejte `CreateLogger` na `ILoggerFactory` instance, jak je znázorněno níže.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

Ve většině případů, ji bude snazší používat `ILogger<T>`, jako v následujícím příkladu.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

To je ekvivalentní volání `CreateLogger` názvem plně kvalifikovaný typ `T`.

## <a name="log-level"></a>Úroveň protokolování

Při každém zápisu do protokolu, zadejte jeho [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel). Určuje úroveň protokolu stupeň závažnosti nebo důležitost. Například můžete například napsat `Information` protokolu, pokud metoda skončí za normálních okolností `Warning` po návratu metody vrátit kód 404 a protokolu `Error` protokolování při zachycení neočekávanou výjimku.

V následujícím příkladu kódu, názvy metod (například `LogWarning`) zadejte úroveň protokolu. První parametr je [protokolu událost s ID](#log-event-id). Je druhý parametr [zpráv šablonu](#log-message-template) se zástupnými symboly pro argument hodnoty podle zbývající parametry metody. Parametry metody jsou vysvětlené podrobněji dále v tomto článku.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Metody protokolu, které obsahují úroveň z názvu metody jsou [rozšiřující metody pro ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions). Na pozadí, tyto metody volat `Log` metodu, která přebírá `LogLevel` parametru. Můžete volat `Log` metoda přímo spíše než jeden z těchto metod rozšíření, ale syntaxe je poměrně složitý. Další informace najdete v tématu [ILogger rozhraní](/dotnet/api/microsoft.extensions.logging.ilogger) a [rozšíření protokolovače zdrojový kód](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).

ASP.NET Core definuje následující [protokolu úrovně](/dotnet/api/microsoft.extensions.logging.loglevel), zde seřazené od nejnižší na nejvyšší závažnost.

* Trasování = 0

  Informace, které je vhodné pouze pro vývojáře ladění chyby. Tyto zprávy mohou obsahovat citlivé aplikaci data a by nemělo být povoleno v produkčním prostředí. *Ve výchozím nastavení zakázané.* Příklad: `Credentials: {"User":"someuser", "Password":"P@ssword"}`

* Ladění = 1

  Informace, která má krátkodobou užitečnost při vývoji a ladění. Příklad: `Entering method Configure with flag set to true.` obvykle neumožňuje `Debug` úroveň zaznamená v produkčním prostředí, pokud řešíte, kvůli velkému počtu protokoly.

* Informace o = 2

  Pro sledování obecný tok z aplikace. Tyto protokoly mají obvykle dlouhodobé některá z hodnot. Příklad: `Request received for path /api/todo`

* Upozornění = 3

  Neobvyklé nebo neočekávané události v aplikaci flow. Ty mohou obsahovat chyby nebo jinými podmínkami, který nezpůsobí zastavení aplikace, ale který možná bude nutné se měl prozkoumat. Zpracované výjimky jsou běžné místo, kde můžete použít `Warning` úrovně protokolování. Příklad: `FileNotFoundException for file quotes.txt.`

* Chyba = 4

  Chyby a výjimky, které nelze zpracovat. Tyto zprávy označují selhání aktuální aktivitu nebo operace (jako je například aktuální požadavek HTTP), ne chybu celou aplikaci. Příklad zprávy protokolu: `Cannot insert record due to duplicate key violation.`

* Kritická = 5

  Chyby, které vyžadují okamžitou pozornost. Příklady: scénářům ztráty dat, volné místo na disku.

Úroveň protokolu můžete určit, kolik výstup protokolu je zapsán do konkrétního úložiště média nebo zobrazení okna. Například v produkčním prostředí může být vhodné všechny protokoly za `Information` úroveň a nižší přejdete na svazek úložiště dat a všechny protokoly za `Warning` úrovně a přejděte do úložiště dat hodnotu vyšší. Během vývoje, můžete obvykle odesílat protokoly `Warning` nebo vyšší závažnost do konzoly. Pokud potřebujete vyřešit, můžete přidat `Debug` úroveň. [Filtrování protokolu](#log-filtering) části dále v tomto článku vysvětluje, jak řídit které úrovně protokolu zpracovává zprostředkovatele.

Zapíše rozhraní ASP.NET Core `Debug` úroveň protokoly pro události rozhraní framework. Příklady protokolu dříve v tomto článku vyloučené protokoly níže `Information` úroveň, proto není `Debug` zobrazila úrovně protokolování. Tady je příklad protokoly konzoly Pokud spustíte ukázkovou aplikaci nakonfigurovat tak, aby zobrazit `Debug` a vyšší protokoly pro zprostředkovatele konzoly.

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

Při každém zápisu do protokolu, můžete zadat *ID události*. Ukázková aplikace dělá to pomocí místně definované `LoggingEvents` třídy:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

ID události je celočíselná hodnota, která vám umožní přidružit sadu protokolovaných událostí mezi sebou. Například protokolu pro přidání položky do nákupního košíku může být událost ID 1000 a protokolu pro dokončení nákupu může být událost ID 1001.

Ve výstupu protokolování může uloženy v poli ID události nebo součástí textové zprávy, v závislosti na poskytovateli. Ladění zprostředkovatele nezobrazí ID událostí, ale konzola poskytovatel je zobrazuje v závorkách za kategorií:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Šablona zprávy protokolu

Pokaždé, když zápisu zprávy do protokolu zadejte šablonu zprávy. Šablona zprávy může být řetězec, nebo může obsahovat pojmenované zástupné symboly, do které argument hodnoty budou vloženy. Šablona není řetězec formátu a zástupné symboly by měly být pojmenovány, ne číslované.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Pořadí zástupné symboly, nikoli jejich názvy, určuje, jaké parametry se používají k zadání jejich hodnot. Pokud máte následující kód:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Výslednou zprávu protokolu vypadá takto:

```
Parameter values: parm1, parm2
```

Rozhraní protokolování zpráv formátování tímto způsobem, aby bylo možné pro protokolování poskytovatele, jak implementovat [sémantického protokolování, označovaného také jako strukturované protokolování](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Protože samotné argumenty jsou předány do systému protokolování, ne jenom šablony formátovaná zpráva poskytovatelé protokolování můžete ukládat hodnoty parametrů jako pole kromě šablonu zprávy. Pokud jste směrování protokolu výstup do služby Azure Table Storage a volání metody protokolovacího nástroje vypadá takto:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Každá entita Azure Table může mít `ID` a `RequestTime` vlastnosti, které zjednodušuje dotazy na data protokolu. Můžete najít všechny protokoly v rámci konkrétní `RequestTime` rozsah, aniž by bylo nutné analyzovat časový limit textové zprávy.

## <a name="logging-exceptions"></a>Protokolování výjimek

Protokolovací nástroj metody mají přetížení, které umožňují předat výjimku, jako v následujícím příkladu:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

Různí poskytovatelé zpracovat informace o výjimce různými způsoby. Tady je příklad výstupu ladění zprostředkovatele z výše uvedeném kódu.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Filtrování protokolu

::: moniker range=">= aspnetcore-2.0"

Můžete zadat minimální úroveň protokolování pro konkrétního zprostředkovatele a kategorie nebo pro všechny poskytovatele nebo všechny kategorie. Všechny protokoly nižší než minimální úroveň nejsou předán tohoto poskytovatele, tak nemusíte získat zobrazení nebo uložené. 

Pokud budete chtít potlačit všechny protokoly, můžete zadat `LogLevel.None` jako minimální úroveň protokolování. Celočíselnou hodnotu `LogLevel.None` je 6, která je vyšší než `LogLevel.Critical` (5).

**Vytvoření pravidla filtru v konfiguraci**

Šablony projektu vytvořit kód, který volá `CreateDefaultBuilder` nastavit protokolování pro poskytovatele konzoly a ladění. `CreateDefaultBuilder` Metoda také nastaví protokolování hledat konfiguraci `Logging` oddílu, psát kód podobný tomuto:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

Konfigurační data Určuje minimální úrovně podle poskytovatele a kategorie, jako v následujícím příkladu:

[!code-json[](index/sample2/appsettings.json)]

Tento dokument JSON vytvoří šest pravidla filtru, jeden pro poskytovatele ladění, čtyři pro zprostředkovatele konzoly a ten, který se vztahuje na všechny poskytovatele. Uvidíte jak později jedním z těchto pravidel je vybrán pro každého zprostředkovatele při `ILogger` je vytvořen objekt.

**Pravidla filtru v kódu**

V kódu, můžete zaregistrovat pravidla filtru, jak je znázorněno v následujícím příkladu:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

Druhá `AddFilter` Určuje zprostředkovatele, který ladění pomocí názvu typu. První `AddFilter` platí pro všechny poskytovatele, protože ji neurčuje, typ zprostředkovatele.

**Jak pravidla filtrování se použijí**

Nastavte `AddFilter`protokolování aplikace (systém souborů) zapnete. Stránka portálu diagnostické protokoly Azure

| Číslo | Zprostředkovatel      | Přejděte streamování protokolů stránku, abyste zobrazili zprávy aplikace.          | Jsou aplikace prostřednictvím přihlášení  rozhraní. |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | Ladit         | Streamování protokolů Azure aplikace na portálu.                          | Informace o       |
| 2      | Konzola       | Microsoft.AspNetCore.Mvc.Razor.Internal | Upozornění           |
| 3      | Konzola       | Microsoft.AspNetCore.Mvc.Razor.Razor    | Ladit             |
| 4      | Konzola       | Microsoft.AspNetCore.Mvc.Razor          | Chyba             |
| 5      | Konzola       | Streamování protokolů Azure aplikace na portálu.                          | Informace o       |
| 6      | Protokolování trasování programu Azure Application Insights | Streamování protokolů Azure aplikace na portálu.                          | Ladit             |
| 7      | Protokolování trasování programu Azure Application Insights | Systém                                  | Ladit             |
| 8      | Ladit         | Microsoft                               | Trasování             |

`ILogger`Application Insights`ILoggerFactory` SDK je schopen shromažďování trasování telemetrických dat z protokolů generovaných prostřednictvím protokolování infrastruktury ASP.NET Core. Další informace najdete v tématu `ILogger`Microsoft/ApplicationInsights-aspnetcore Wiki: protokolování. Vysoce výkonné protokolování pomocí LoggerMessage

Následující požadovaný algoritmus se používá pro každého zprostředkovatele při `ILogger` se vytvoří pro danou kategorii:

* Vyberte všechna pravidla, které odpovídají zprostředkovateli nebo její alias. Pokud nejsou nalezeny, vyberte všechna pravidla s poskytovatelem služby prázdný.
* Z výsledků v předchozím kroku vyberte pravidla s nejdelší odpovídající předpona kategorie. Pokud nejsou nalezeny, vyberte všechna pravidla, které nechcete zadat kategorii.
* Pokud je vybraných víc pravidel trvat **poslední** jeden.
* Pokud jsou vybraná žádná pravidla, použijte `MinimumLevel`.

Předpokládejme například, že máte předchozí seznam pravidel a můžete vytvořit `ILogger` objektu pro kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":

* Ladění zprostředkovatele platí pravidla 1, 6 a 8. Pravidlo 8 není nejvíce specifické, tak, aby se byla vybrána.
* Pro poskytovatele konzoly platí pravidla 3, 4, 5 a 6. Pravidlo 3 není nejvíce specifické.

Při vytváření protokolů pomocí `ILogger` pro kategorii "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" protokoly z `Trace` úrovně a vyšší budou moct ladění zprostředkovatele a protokoly `Debug` úrovně a vyšší přejde k poskytovateli konzoly.

**Aliasy poskytovatele**

Název typu můžete použít k zadání zprostředkovatele v konfiguraci, ale každý poskytovatel definuje v kratší *alias* , která se snadněji používá. Pro předdefinované zprostředkovatele použijte následující aliasy:

- Konzola
- Ladit
- Protokol událostí
- AzureAppServices
- TraceSource
- EventSource

**Výchozí minimální úroveň**

Je k dispozici minimální úroveň nastavení, která se projeví pouze v případě, že žádná pravidla z konfigurace nebo kódu platí pro daného poskytovatele a kategorii. Následující příklad ukazuje, jak nastavit minimální úroveň:

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

Pokud není explicitně nastavena na minimální úroveň, výchozí hodnota je `Information`, což znamená, že `Trace` a `Debug` protokoly jsou ignorovány.

**Funkce filtru**

Můžete napsat kód ve funkci filtru použít pravidla filtrování. Pro všechny poskytovatele a kategorie, které nemají přiřazené konfigurace nebo kódu pravidla je vyvolána funkce filtru. Kód ve funkci má přístup k typ zprostředkovatele, kategorii a úroveň protokolu se rozhodnout, zda mají být protokolovány zprávu. Příklad:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Někteří poskytovatelé protokolování umožňují zadat při protokoly by měly být zapsána do úložiště média nebo ignorovat podle úroveň protokolu a kategorie.

`AddConsole` a `AddDebug` rozšiřující metody poskytují přetížení, které umožňují předat kritéria filtrování. Následující ukázkový kód způsobí, že konzola poskytovatel ignorovat protokolech níže `Warning` úroveň, při ladění zprostředkovatele ignoruje protokoly, které vytvoří rozhraní framework.

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

`AddEventLog` Metoda má přetížení přijímající `EventLogSettings` instanci, která může obsahovat funkce filtrování v jeho `Filter` vlastnost. Zprostředkovatel TraceSource neposkytuje, některý z těchto přetížení, protože jeho úroveň protokolování a další parametry jsou založené na `SourceSwitch` a `TraceListener` používá.

Můžete nastavit pravidla filtrování pro všech zprostředkovatelů, které jsou registrovány `ILoggerFactory` instance pomocí `WithFilter` – metoda rozšíření. Následující příklad omezuje framework protokoly (kategorie začíná řetězcem "Microsoft" nebo "Systém") a upozornění přičemž umožníte aplikaci protokolu na úroveň ladění.

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Pokud chcete zabránit zápisu pro danou kategorii všechny protokoly pomocí filtrování, můžete zadat `LogLevel.None` jako minimální úroveň protokolování pro dané kategorie. Celočíselnou hodnotu `LogLevel.None` je 6, která je vyšší než `LogLevel.Critical` (5).

`WithFilter` – Metoda rozšíření poskytuje [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) balíček NuGet. Metoda vrátí nový `ILoggerFactory` zaregistrovaný poskytovatel instanci, která bude filtrovat zprávy protokolu, který je předán všichni poskytovatelé protokolovacího nástroje. To nemá vliv na jiné `ILoggerFactory` instance, včetně původní `ILoggerFactory` instance.

::: moniker-end

## <a name="log-scopes"></a>Protokol obory

Můžete seskupit sadu logické operace v rámci *oboru* aby bylo možné připojit stejná data pro každý protokol, který je vytvořen jako součást této sady. Například může být vhodné každý protokol vytvořených jako součást zpracování transakce zahrnují ID transakce.

Obor je `IDisposable` typ, který je vrácen [ILogger.BeginScope&lt;TState&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) metoda a bude trvat, dokud je uvolněn. Použijete zabalení vaší protokolovací nástroj se volá v oboru `using` blokovat, jak je znázorněno zde:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Následující kód umožní obory pro zprostředkovatele konzoly:

::: moniker range="> aspnetcore-2.0"

*Soubor program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Konfigurace `IncludeScopes` možnost protokolovací nástroj konzoly je nutné povolit protokolování na základě oboru.
>
> `IncludeScopes` můžete nakonfigurovat přes *appsettings* konfigurační soubory. Další informace najdete v tématu [konfigurační soubor nastavení](#settings-file-configuration) oddílu.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

*Soubor program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Konfigurace `IncludeScopes` možnost protokolovací nástroj konzoly je nutné povolit protokolování na základě oboru.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

*Startup.cs*:

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

Každé zprávě protokolu obsahuje rozsahem informací:

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>Poskytovatelé vestavěné protokolování

ASP.NET Core se celá dodává následující zprostředkovatele:

* [Console](#console-provider)
* [Ladění](#debug-provider)
* [EventSource](#eventsource-provider)
* [EventLog](#windows-eventlog-provider)
* [TraceSource](#tracesource-provider)
* [Azure App Service](#azure-app-service-provider)

### <a name="console-provider"></a>Konzola zprostředkovatele

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) balíček zprostředkovatele odešle výstup protokolu konzoly. 

::: moniker range=">= aspnetcore-2.0"


```csharp
logging.AddConsole()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole()
```

[Přetížení AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) umožňují předáním minimální úroveň protokolování, funkce filtru a logickou hodnotu, která označuje, zda jsou obory. Další možností je a zajistěte tak předání `IConfiguration` objektu, který můžete určit podporu obory a úrovní protokolování. 

Pokud uvažujete o poskytovateli konzoly pro použití v produkčním prostředí, mějte na paměti, že má významný dopad na výkon.

Když vytvoříte nový projekt v sadě Visual Studio `AddConsole` metoda vypadá takto:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Tento kód odkazuje `Logging` část *appSettings.json* souboru:

[!code-json[](index/sample//appsettings.json)]

Nastavení zobrazené protokoly framework limit upozornění zároveň umožní aplikaci k přihlášení na úrovni ladění, jak je vysvětleno v [filtrování protokolu](#log-filtering) oddílu. Další informace najdete v tématu [konfigurace](xref:fundamentals/configuration/index).

::: moniker-end

### <a name="debug-provider"></a>Ladění zprostředkovatele

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) balíček zprostředkovatele zapíše výstup protokolu pomocí [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) třídy (`Debug.WriteLine` volání metody).

V systému Linux, tento zprostředkovatel zapisuje protokoly do */var/log/message*.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug()
```

[Přetížení AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) umožňují předáním minimální úroveň protokolování nebo funkce filtru.

::: moniker-end

### <a name="eventsource-provider"></a>Zprostředkovatel EventSource

Pro aplikace, které cílí ASP.NET Core 1.1.0 nebo vyšší, [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) balíček zprostředkovatele můžete implementovat trasování událostí. Na Windows, používá [trasování událostí pro Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803). Zprostředkovatel je multiplatformní, ale nejsou žádná událost kolekce a zobrazení nástroje ještě pro Linux nebo macOS. 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger()
```

::: moniker-end

Je dobrým způsobem, jak shromažďovat a zobrazovat protokoly použít [nástroje PerfView](https://github.com/Microsoft/perfview). Existují jiné nástroje pro prohlížení protokolů trasování událostí pro Windows, ale PerfView poskytuje nejlepší prostředí pro práci s událostmi trasování událostí pro Windows, protože ho vygeneroval technologie ASP.NET. 

Ke konfiguraci PerfView pro shromažďování události zapsané podle tohoto zprostředkovatele, přidejte řetězec `*Microsoft-Extensions-Logging` k **dalších poskytovatelů** seznamu. (Nenechte si ujít hvězdičku na začátku řetězce.)

![Další zprostředkovatelé Perfview](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Poskytovatel protokolu událostí Windows

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) balíček zprostředkovatele odesílá výstup protokolu do protokolu událostí Windows.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog()
```

[Přetížení AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) umožňují předáním `EventLogSettings` nebo minimální úroveň protokolování.

::: moniker-end

### <a name="tracesource-provider"></a>TraceSource poskytovatele

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) používá balíček zprostředkovatele [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) knihovny a poskytovatelů.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

[Přetížení AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) umožňují předáním přepínač zdroje a naslouchací proces trasování.

K používání tohoto poskytovatele, má aplikace spouštět rozhraní .NET Framework (spíše než .NET Core). Zprostředkovatel umožňuje směrovat zprávy širokou škálu [naslouchacích procesů](/dotnet/framework/debug-trace-profile/trace-listeners), například [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) použít v ukázkové aplikaci.

Následující příklad nastaví `TraceSource` zprostředkovatele, který zaznamenává `Warning` a vyšší zprávu do okna konzoly.

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a>Zprostředkovatel služby Azure App Service

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) balíček zprostředkovatele zapisuje protokoly do textových souborů v systému souborů aplikace služby Azure App Service a na [úložiště objektů blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) v účtu služby Azure Storage. Zprostředkovatel je k dispozici pouze u aplikací určených pro ASP.NET Core 1.1 nebo novější.

::: moniker range=">= aspnetcore-2.0"

Pokud cílí na .NET Core, nemusíte instalovat balíček zprostředkovatele nebo explicitně volat [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics). Zprostředkovatel je automaticky dostupný pro aplikace, při nasazení aplikace do služby Azure App Service.

Pokud se zaměřujete na rozhraní .NET Framework, přidejte do projektu balíček zprostředkovatele a vyvolání `AddAzureWebAppDiagnostics`:

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

[AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) přetížení vám umožní předat v [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) pomocí kterého můžete přepsat výchozí nastavení, jako je například protokolování výstupu šablony, názvu objektu blob a souboru omezení velikosti. (*Výstupu šablony* je zpráv šablonu, která se použije pro všechny protokoly nad ten, který zadáte při volání `ILogger` metoda.)

::: moniker-end

Když nasadíte aplikaci služby App Service, aplikace respektuje nastavení v [diagnostické protokoly](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) část **služby App Service** stránky na webu Azure portal. Když tato nastavení jsou aktualizovány, změny se projeví okamžitě bez nutnosti restartování nebo opětovné nasazení aplikace.

![Nastavení protokolování Azure](index/_static/azure-logging-settings.png)

Výchozím umístěním pro soubory protokolů je *D:\\domácí\\LogFiles\\aplikace* složky a výchozí název souboru je *diagnostiky yyyymmdd.txt*. Výchozí limit velikosti souboru je 10 MB a výchozí maximální počet souborů, které uchovávají se 2. Výchozí název objektu blob je *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*. Další informace o výchozím chování najdete v tématu [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).

Zprostředkovatel funguje pouze v případě projektu běží v prostředí Azure. Nemá žádný vliv, pokud projekt je spuštěn místně&mdash;nelze zapsat do místních souborů nebo místním vývojovým úložištěm objektů BLOB.

## <a name="third-party-logging-providers"></a>Zprostředkovatele přihlášení třetí strany

Rozhraní protokolování třetích stran, které pracují s ASP.NET Core:

* [elmah.IO](https://elmah.io/) ([úložiště GitHub se vzorovými](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([úložiště GitHub se vzorovými](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](http://jsnlog.com/) ([úložiště GitHub se vzorovými](https://github.com/mperdeck/jsnlog))
* [Loggr](http://loggr.net/) ([úložiště GitHub se vzorovými](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](http://nlog-project.org/) ([úložiště GitHub se vzorovými](https://github.com/NLog/NLog.Extensions.Logging))
* [Serilog](https://serilog.net/) ([úložiště GitHub se vzorovými](https://github.com/serilog/serilog-extensions-logging))

Můžete provádět některé rozhraní třetích stran [sémantického protokolování, označovaného také jako strukturované protokolování](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Použití rozhraní třetích stran je podobný pomocí jedné z předdefinovaných poskytovatelů:

1. Přidání balíčku NuGet do projektu.
1. Volání metody rozšíření na `ILoggerFactory`.

Další informace najdete v dokumentaci každého rozhraní.

## <a name="azure-log-streaming"></a>Streamování protokolů Azure

Streamování protokolů Azure umožňuje zobrazení protokolu aktivit v reálném čase: 

* Aplikační server
* Webový server
* Trasování neúspěšných žádostí

Postup konfigurace, streamování protokolů Azure:

* Přejděte **diagnostické protokoly** stránky na stránce portálu vaší aplikace
* Nastavte **protokolování aplikace (systém souborů)** zapnete.

![Stránka portálu diagnostické protokoly Azure](index/_static/azure-diagnostic-logs.png)

Přejděte **streamování protokolů** stránku, abyste zobrazili zprávy aplikace. Jsou aplikace prostřednictvím přihlášení `ILogger` rozhraní.

![Streamování protokolů Azure aplikace na portálu.](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a>Protokolování trasování programu Azure Application Insights

[Application Insights](https://azure.microsoft.com/services/application-insights/) SDK je schopen shromažďování trasování telemetrických dat z protokolů generovaných prostřednictvím protokolování infrastruktury ASP.NET Core. Další informace najdete v tématu [Microsoft/ApplicationInsights-aspnetcore Wiki: protokolování](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).

## <a name="additional-resources"></a>Další zdroje

[Vysoce výkonné protokolování pomocí LoggerMessage](xref:fundamentals/logging/loggermessage)
