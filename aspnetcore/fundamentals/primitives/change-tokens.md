---
title: Detekovat změny s tokeny změn v ASP.NET Core
author: guardrex
description: Další informace o použití změn tokeny ke sledování změn.
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 9fd76ea1c6d746ecf1897c70ee719ea4f4517872
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729152"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>Detekovat změny s tokeny změn v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

A *změnit token* je použít ke sledování změn stavební blok pro obecné účely, nízké úrovně.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>IChangeToken rozhraní

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) šíří oznámení, že došlo ke změně. `IChangeToken` se nachází v [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) oboru názvů. Pro aplikace, které nepoužívají [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) metapackage, odkaz [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) balíček NuGet v souboru projektu.

`IChangeToken` má dvě vlastnosti:

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indikovat, pokud je token proaktivně vyvolá zpětná volání. Pokud `ActiveChangedCallbacks` je nastaven na `false`, označuje se nikdy zpětné volání, a aplikace se musí dotazovat `HasChanged` změny. Je také možné pro token nikdy zrušit Pokud dojde k žádným změnám nebo základní naslouchací proces pro změny je zrušen nebo zakázána.
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) získá hodnotu, která určuje, pokud došlo ke změně.

Rozhraní má jednu metodu [RegisterChangeCallback (akce&lt;objekt&gt;, objekt)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), čímž registruje zpětné volání, které je voláno, když došlo ke změně token. `HasChanged` musí být nastaven před vyvoláním zpětné volání.

## <a name="changetoken-class"></a>ChangeToken – třída

`ChangeToken` slouží k šíření oznámení, že došlo ke změně statická třída. `ChangeToken` se nachází v [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) oboru názvů. Pro aplikace, které nepoužívají [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) metapackage, odkaz [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) balíček NuGet v souboru projektu.

`ChangeToken` [Při změně (Func&lt;IChangeToken&gt;, akce)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) metoda registruje `Action` volat při každé změně token:

* `Func<IChangeToken>` vytvoří token.
* `Action` je volána, když se změní token.

`ChangeToken` má [při změně&lt;TState&gt;(Func&lt;IChangeToken&gt;, akce&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) přetížení, které nepřijímá další `TState` parametr, který je předán do tokenu příjemce `Action`.

`OnChange` Vrátí [IDisposable](/dotnet/api/system.idisposable). Volání metody [Dispose](/dotnet/api/system.idisposable.dispose) zastaví tokenu z čekání na další změny a uvolní prostředky, je token.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Příklad používá změnu tokenů v ASP.NET Core

Změna tokeny se používají v hlavní oblasti ASP.NET Core změny provedené u objektů monitorování:

* Pro monitorování změny souborů, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)na [sledovat](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) metoda vytvoří `IChangeToken` určité soubory nebo složku ke sledování.
* `IChangeToken` tokeny mohou být přidány do záznamů mezipaměti určených k aktivaci vyřazení procesu mezipaměti při změně.
* Pro `TOptions` změní, výchozí [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementace [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) má přetížení, které přijímá jeden nebo více [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1)instance. Každá instance vrátí `IChangeToken` pro registraci zpětné volání oznámení změn pro možnosti sledování změn.

## <a name="monitoring-for-configuration-changes"></a>Monitorování pro změny konfigurace

Ve výchozím nastavení, použijte šablony ASP.NET Core [konfigurační soubory JSON](xref:fundamentals/configuration/index#json-configuration) (*appSettings.JSON určený*, *appsettings. Development.JSON*, a *appsettings. Production.JSON*) se načíst konfigurační nastavení aplikace.

Tyto soubory jsou konfigurováni pomocí [AddJsonFile (IConfigurationBuilder, řetězec, logická hodnota, logická hodnota)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) rozšiřující metody na [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) který přijme `reloadOnChange` parametr (ASP.NET Základní 1.1 nebo novější). `reloadOnChange` Označuje, pokud by měl znovu načíst konfiguraci na změny souboru. Toto nastavení najdete v článku [tomuto webovému hostiteli](/dotnet/api/microsoft.aspnetcore.webhost) pohodlí metoda [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder):

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

Konfigurace na základě souborů je reprezentována [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource). `FileConfigurationSource` používá [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) sledování souborů.

Ve výchozím nastavení `IFileMonitor` zajišťuje [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), které používá [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) ke sledování změn konfigurace.

Ukázková aplikace ukazuje dva implementace pro sledování změn konfigurace. Pokud buď *appSettings.JSON určený* změny souborů nebo prostředí verzi souboru se změní, každá implementace provede vlastní kód. Ukázková aplikace zapíše zprávu do konzoly.

Konfigurační soubor `FileSystemWatcher` můžete aktivovat více tokenu zpětných volání pro změnu jedna konfigurační soubor. Implementace tohoto příkladu chrání před tento problém kontrolou hodnoty hash souboru na konfigurační soubory. Kontrola hodnoty hash souboru zajišťuje, že alespoň jeden z konfiguračních souborů se změnila před spuštěním vlastního kódu. Příklad používá algoritmu hash SHA1 souboru (*Utilities/Utilities.cs*):

   [!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   Opakovaný pokus je implementováno s exponenciální back vypnout. Zkuste znovu je přítomen, protože zámek souborů může dojít, která zabraňuje dočasně computing novou hodnotu hash na jeden ze souborů.

### <a name="simple-startup-change-token"></a>Token změny jednoduché spuštění

Zaregistrovat tokenu příjemce `Action` zpětné volání pro upozornění na změnu konfigurace opětovného načtení tokenu (*Startup.cs*):

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()` poskytuje token. Zpětné volání je `InvokeChanged` metoda:

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet3)]

`state` Zpětného volání, které se používá k předávat `IHostingEnvironment`. To je užitečné k určení správného *appsettings* konfigurační soubor JSON pro monitorování, *appsettings.&lt; Prostředí&gt;.json*. Hodnoty hash souboru se používají k zabránit `WriteConsole` příkaz spuštění více než jednou. z důvodu více tokenu zpětná volání, když konfiguračního souboru změnil pouze jednou.

Tento systém se spustí, pokud aplikace běží a nedá se zakázat uživatelem.

### <a name="monitoring-configuration-changes-as-a-service"></a>Sledování změn konfigurace jako služby

Implementuje ukázku:

* Základní spuštění tokenu monitorování.
* Monitorování jako služba.
* Mechanismus pro povolení a zákaz monitorování.

Vytvoří vzorovou `IConfigurationMonitor` rozhraní (*Extensions/ConfigurationMonitor.cs*):

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

Konstruktor implementované třídy `ConfigurationMonitor`, zaregistruje zpětné volání upozornění na změny:

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` poskytuje token. `InvokeChanged` je metoda zpětného volání. `state` v této instanci je odkaz na `IConfigurationMonitor` instanci, která se používá pro přístup k monitorování stavu. Dvě vlastnosti se používají:

* `MonitoringEnabled` Určuje, pokud zpětné volání se budou spouštět jeho vlastní kód.
* `CurrentState` Popisuje aktuální monitorování stavu pro použití v uživatelském rozhraní.

`InvokeChanged` Metoda je podobná starší přístup, s výjimkou, že:

* Nefunguje jeho kód, pokud `MonitoringEnabled` je `true`.
* Poznámky k aktuální `state` v jeho `WriteConsole` výstup.

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Instance `ConfigurationMonitor` je zaregistrován jako služba v `ConfigureServices` z *Startup.cs*:

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet1)]

Indexovou stránku nabízí uživatelského ovládacího prvku přes monitorování konfigurací. Instance `IConfigurationMonitor` je vloženy do `IndexModel`:

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

Tlačítko povolí nebo zakáže monitorování:

[!code-cshtml[](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

Když `OnPostStartMonitoring` je aktivována, je zapnuto monitorování a aktuální stav je vymazán. Když `OnPostStopMonitoring` je aktivována, monitorování je zakázáno a stav nastaven tak, aby odrážela, že monitorování neprobíhá.

## <a name="monitoring-cached-file-changes"></a>Monitorování změn souborů uložených v mezipaměti

Obsah souboru může být uložená v mezipaměti v paměti pomocí [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache). Ukládání do mezipaměti v paměti je podrobněji popsaná [mezipaměti v paměti](xref:performance/caching/memory) tématu. Bez nutnosti převádět další kroky, jako je popsáno níže, implementace *zastaralé* (zastaralé) data jsou vrácena z mezipaměti, pokud se změní zdrojová data.

Bez zohlednění stav v mezipaměti zdrojového souboru při obnovování [klouzavé vypršení platnosti](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) období vede k zastaralých data do mezipaměti. Každý požadavek pro data obnovuje klouzavou dobu vypršení platnosti, ale soubor nikdy znovu načíst do mezipaměti. Všechny funkce aplikací, které používají obsah uložený v mezipaměti v souboru se vztahují pravděpodobně přijetí starý obsah.

Používání tokenů změn v souboru ukládání do mezipaměti scénář zabraňuje zastaralých souborů obsahu v mezipaměti. Ukázková aplikace ukazuje implementaci tohoto přístupu.

Příklad používá `GetFileContent` na:

* Vrátí obsah souboru.
* Implementujte algoritmus zkuste to znovu s exponenciální back vypnout pro případy, kdy uzamčení souboru dočasně znemožňuje soubor při čtení.

*Utilities/Utilities.cs*:

[!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

A `FileService` se vytvoří pro zpracování vyhledávání v mezipaměti souborů. `GetFileContent` Volání metody služby se pokusí získat obsah souboru z mezipaměti v paměti a obnoví v něm volajícímu (*Services/FileService.cs*).

Pokud obsah uložený v mezipaměti není nalezen pomocí klíče mezipaměti, budou provedeny následující akce:

1. Obsah souboru se získávají pomocí `GetFileContent`.
1. Změna token se získávají z poskytovatele souborů s [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch). Zpětné volání je token se aktivuje, když je změny souboru.
1. Obsah souboru je uložené v mezipaměti s [klouzavé vypršení platnosti](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) období. Token změny je připojené k [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) vyřazení položky mezipaměti, pokud se soubor změní, když se uloží do mezipaměti.

[!code-csharp[](change-tokens/sample/Services/FileService.cs?name=snippet1)]

`FileService` Je zaregistrován v kontejneru služby společně s paměti ukládání do mezipaměti služby (*Startup.cs*):

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet4)]

Model stránka načte obsah souboru pomocí služby (*Pages/Index.cshtml.cs*):

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>CompositeChangeToken – třída

Pro představující jednu nebo více `IChangeToken` instancí v jednom objektu, použijte [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) třídy.

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        { 
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

`HasChanged` na složené tokenu sestavy `true` Pokud žádné reprezentované token `HasChanged` je `true`. `ActiveChangeCallbacks` na složené tokenu sestavy `true` Pokud žádné reprezentované token `ActiveChangeCallbacks` je `true`. Pokud dojde k události více souběžných změny, je vyvolána zpětného volání kompozitních změn přesně jednou.

## <a name="additional-resources"></a>Další zdroje

* [Mezipaměť v paměti](xref:performance/caching/memory)
* [Práce s distribuovanou mezipamětí](xref:performance/caching/distributed)
* [Ukládání odpovědí do mezipaměti](xref:performance/caching/response)
* [Middleware pro ukládání odpovědí do mezipaměti](xref:performance/caching/middleware)
* [Uložení pomocné rutiny značky do mezipaměti](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Pomocná rutina značek v distribuované mezipaměti](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
