---
title: "Detekovat změny s tokeny změn v ASP.NET Core"
author: guardrex
description: "Další informace o použití změn tokeny ke sledování změn."
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 94bf356fcbfab3930804485c1b65e4a0f4c52b8e
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/31/2018
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="ab21c-103">Detekovat změny s tokeny změn v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab21c-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="ab21c-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ab21c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ab21c-105">A *změnit token* je použít ke sledování změn stavební blok pro obecné účely, nízké úrovně.</span><span class="sxs-lookup"><span data-stu-id="ab21c-105">A *change token* is a general-purpose, low-level building block used to track changes.</span></span>

<span data-ttu-id="ab21c-106">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ab21c-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="ab21c-107">IChangeToken rozhraní</span><span class="sxs-lookup"><span data-stu-id="ab21c-107">IChangeToken interface</span></span>

<span data-ttu-id="ab21c-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) šíří oznámení, že došlo ke změně.</span><span class="sxs-lookup"><span data-stu-id="ab21c-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propagates notifications that a change has occurred.</span></span> <span data-ttu-id="ab21c-109">`IChangeToken`se nachází v [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="ab21c-109">`IChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="ab21c-110">Pro aplikace, které nepoužívají [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, odkaz [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) balíček NuGet v souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="ab21c-110">For apps that don't use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="ab21c-111">`IChangeToken`má dvě vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="ab21c-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="ab21c-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indikovat, pokud je token proaktivně vyvolá zpětná volání.</span><span class="sxs-lookup"><span data-stu-id="ab21c-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="ab21c-113">Pokud `ActiveChangedCallbacks` je nastaven na `false`, označuje se nikdy zpětné volání, a aplikace se musí dotazovat `HasChanged` změny.</span><span class="sxs-lookup"><span data-stu-id="ab21c-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="ab21c-114">Je také možné pro token nikdy zrušit Pokud dojde k žádným změnám nebo základní naslouchací proces pro změny je zrušen nebo zakázána.</span><span class="sxs-lookup"><span data-stu-id="ab21c-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="ab21c-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) získá hodnotu, která určuje, pokud došlo ke změně.</span><span class="sxs-lookup"><span data-stu-id="ab21c-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) gets a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="ab21c-116">Rozhraní má jednu metodu [RegisterChangeCallback (akce&lt;objekt&gt;, objekt)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), čímž registruje zpětné volání, které je voláno, když došlo ke změně token.</span><span class="sxs-lookup"><span data-stu-id="ab21c-116">The interface has one method, [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="ab21c-117">`HasChanged`musí být nastaven před vyvoláním zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="ab21c-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="ab21c-118">ChangeToken – třída</span><span class="sxs-lookup"><span data-stu-id="ab21c-118">ChangeToken class</span></span>

<span data-ttu-id="ab21c-119">`ChangeToken`slouží k šíření oznámení, že došlo ke změně statická třída.</span><span class="sxs-lookup"><span data-stu-id="ab21c-119">`ChangeToken` is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="ab21c-120">`ChangeToken`se nachází v [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="ab21c-120">`ChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="ab21c-121">Pro aplikace, které nepoužívají [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, odkaz [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) balíček NuGet v souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="ab21c-121">For apps that don't use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="ab21c-122">`ChangeToken` [Při změně (Func&lt;IChangeToken&gt;, akce)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) metoda registruje `Action` volat při každé změně token:</span><span class="sxs-lookup"><span data-stu-id="ab21c-122">The `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) method registers an `Action` to call whenever the token changes:</span></span>
* <span data-ttu-id="ab21c-123">`Func<IChangeToken>`vytvoří token.</span><span class="sxs-lookup"><span data-stu-id="ab21c-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="ab21c-124">`Action`je volána, když se změní token.</span><span class="sxs-lookup"><span data-stu-id="ab21c-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="ab21c-125">`ChangeToken`má [při změně&lt;TState&gt;(Func&lt;IChangeToken&gt;, akce&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) přetížení, které nepřijímá další `TState`parametr, který je předán do tokenu příjemce `Action`.</span><span class="sxs-lookup"><span data-stu-id="ab21c-125">`ChangeToken` has an [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) overload that takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="ab21c-126">`OnChange`Vrátí [IDisposable](/dotnet/api/system.idisposable).</span><span class="sxs-lookup"><span data-stu-id="ab21c-126">`OnChange` returns an [IDisposable](/dotnet/api/system.idisposable).</span></span> <span data-ttu-id="ab21c-127">Volání metody [Dispose](/dotnet/api/system.idisposable.dispose) zastaví tokenu z čekání na další změny a uvolní prostředky, je token.</span><span class="sxs-lookup"><span data-stu-id="ab21c-127">Calling [Dispose](/dotnet/api/system.idisposable.dispose) stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="ab21c-128">Příklad používá změnu tokenů v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab21c-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="ab21c-129">Změna tokeny se používají v hlavní oblasti ASP.NET Core změny provedené u objektů monitorování:</span><span class="sxs-lookup"><span data-stu-id="ab21c-129">Change tokens are used in prominent areas of ASP.NET Core monitoring changes to objects:</span></span>

* <span data-ttu-id="ab21c-130">Pro monitorování změny souborů, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)na [sledovat](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) metoda vytvoří `IChangeToken` určité soubory nebo složku ke sledování.</span><span class="sxs-lookup"><span data-stu-id="ab21c-130">For monitoring changes to files, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)'s [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="ab21c-131">`IChangeToken`tokeny mohou být přidány do záznamů mezipaměti určených k aktivaci vyřazení procesu mezipaměti při změně.</span><span class="sxs-lookup"><span data-stu-id="ab21c-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="ab21c-132">Pro `TOptions` změní, výchozí [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementace [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) má přetížení, které přijímá jeden nebo více [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1)instance.</span><span class="sxs-lookup"><span data-stu-id="ab21c-132">For `TOptions` changes, the default [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementation of [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) has an overload that accepts one or more [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) instances.</span></span> <span data-ttu-id="ab21c-133">Každá instance vrátí `IChangeToken` pro registraci zpětné volání oznámení změn pro možnosti sledování změn.</span><span class="sxs-lookup"><span data-stu-id="ab21c-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitoring-for-configuration-changes"></a><span data-ttu-id="ab21c-134">Monitorování pro změny konfigurace</span><span class="sxs-lookup"><span data-stu-id="ab21c-134">Monitoring for configuration changes</span></span>

<span data-ttu-id="ab21c-135">Ve výchozím nastavení, použijte šablony ASP.NET Core [konfigurační soubory JSON](xref:fundamentals/configuration/index#json-configuration) (*appSettings.JSON určený*, *appsettings. Development.JSON*, a *appsettings. Production.JSON*) se načíst konfigurační nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="ab21c-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="ab21c-136">Tyto soubory jsou konfigurováni pomocí [AddJsonFile (IConfigurationBuilder, řetězec, logická hodnota, logická hodnota)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) rozšiřující metody na [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) který přijme `reloadOnChange` parametr (ASP.NET Základní 1.1 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="ab21c-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) extension method on [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) that accepts a `reloadOnChange` parameter (ASP.NET Core 1.1 and later).</span></span> <span data-ttu-id="ab21c-137">`reloadOnChange`Označuje, pokud by měl znovu načíst konfiguraci na změny souboru.</span><span class="sxs-lookup"><span data-stu-id="ab21c-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="ab21c-138">Toto nastavení najdete v článku [tomuto webovému hostiteli](/dotnet/api/microsoft.aspnetcore.webhost) pohodlí metoda [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ([odkaz na zdroj](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193)):</span><span class="sxs-lookup"><span data-stu-id="ab21c-138">See this setting in the [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) convenience method [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ([reference source](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193)):</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

<span data-ttu-id="ab21c-139">Konfigurace na základě souborů je reprezentována [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span><span class="sxs-lookup"><span data-stu-id="ab21c-139">File-based configuration is represented by [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span></span> <span data-ttu-id="ab21c-140">`FileConfigurationSource`používá [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) ([odkaz na zdroj](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)) ke sledování souborů.</span><span class="sxs-lookup"><span data-stu-id="ab21c-140">`FileConfigurationSource` uses [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) ([reference source](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)) to monitor files.</span></span>

<span data-ttu-id="ab21c-141">Ve výchozím nastavení `IFileMonitor` zajišťuje [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ([odkaz na zdroj](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82)), které používá [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) pro monitorování konfiguračního souboru změny.</span><span class="sxs-lookup"><span data-stu-id="ab21c-141">By default, the `IFileMonitor` is provided by a [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ([reference source](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82)), which uses [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) to monitor for configuration file changes.</span></span>

<span data-ttu-id="ab21c-142">Ukázková aplikace ukazuje dva implementace pro sledování změn konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ab21c-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="ab21c-143">Pokud buď *appSettings.JSON určený* změny souborů nebo prostředí verzi souboru se změní, každá implementace provede vlastní kód.</span><span class="sxs-lookup"><span data-stu-id="ab21c-143">If either the *appsettings.json* file changes or the Environment version of the file changes, each implementation executes custom code.</span></span> <span data-ttu-id="ab21c-144">Ukázková aplikace zapíše zprávu do konzoly.</span><span class="sxs-lookup"><span data-stu-id="ab21c-144">The sample app writes a message to the console.</span></span>

<span data-ttu-id="ab21c-145">Konfigurační soubor `FileSystemWatcher` můžete aktivovat více tokenu zpětných volání pro změnu jedna konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="ab21c-145">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="ab21c-146">Implementace tohoto příkladu chrání před tento problém kontrolou hodnoty hash souboru na konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="ab21c-146">The sample's implementation guards against this problem by checking file hashes on the configuration files.</span></span> <span data-ttu-id="ab21c-147">Kontrola hodnoty hash souboru zajišťuje, že alespoň jeden z konfiguračních souborů se změnila před spuštěním vlastního kódu.</span><span class="sxs-lookup"><span data-stu-id="ab21c-147">Checking file hashes ensures that at least one of the configuration files has changed before running the custom code.</span></span> <span data-ttu-id="ab21c-148">Příklad používá algoritmu hash SHA1 souboru (*Utilities/Utilities.cs*):</span><span class="sxs-lookup"><span data-stu-id="ab21c-148">The sample uses SHA1 file hashing (*Utilities/Utilities.cs*):</span></span>

   [!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   <span data-ttu-id="ab21c-149">Opakovaný pokus je implementováno s exponenciální back vypnout.</span><span class="sxs-lookup"><span data-stu-id="ab21c-149">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="ab21c-150">Zkuste znovu je přítomen, protože zámek souborů může dojít, která zabraňuje dočasně computing novou hodnotu hash na jeden ze souborů.</span><span class="sxs-lookup"><span data-stu-id="ab21c-150">The re-try is present because file locking may occur that temporarily prevents computing a new hash on one of the files.</span></span>

### <a name="simple-startup-change-token"></a><span data-ttu-id="ab21c-151">Token změny jednoduché spuštění</span><span class="sxs-lookup"><span data-stu-id="ab21c-151">Simple startup change token</span></span>

<span data-ttu-id="ab21c-152">Zaregistrovat tokenu příjemce `Action` zpětné volání pro upozornění na změnu konfigurace opětovného načtení tokenu (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="ab21c-152">Register a token consumer `Action` callback for change notifications to the configuration reload token (*Startup.cs*):</span></span>

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="ab21c-153">`config.GetReloadToken()`poskytuje token.</span><span class="sxs-lookup"><span data-stu-id="ab21c-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="ab21c-154">Zpětné volání je `InvokeChanged` metoda:</span><span class="sxs-lookup"><span data-stu-id="ab21c-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet3)]

<span data-ttu-id="ab21c-155">`state` Zpětného volání, které se používá k předávat `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="ab21c-155">The `state` of the callback is used to pass in the `IHostingEnvironment`.</span></span> <span data-ttu-id="ab21c-156">To je užitečné k určení správného *appsettings* konfigurační soubor JSON pro monitorování, *appsettings.&lt; Prostředí&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="ab21c-156">This is useful to determine the correct *appsettings* configuration JSON file to monitor, *appsettings.&lt;Environment&gt;.json*.</span></span> <span data-ttu-id="ab21c-157">Hodnoty hash souboru se používají k zabránit `WriteConsole` příkaz spuštění více než jednou. z důvodu více tokenu zpětná volání, když konfiguračního souboru změnil pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="ab21c-157">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="ab21c-158">Tento systém se spustí, pokud aplikace běží a nedá se zakázat uživatelem.</span><span class="sxs-lookup"><span data-stu-id="ab21c-158">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitoring-configuration-changes-as-a-service"></a><span data-ttu-id="ab21c-159">Sledování změn konfigurace jako služby</span><span class="sxs-lookup"><span data-stu-id="ab21c-159">Monitoring configuration changes as a service</span></span>

<span data-ttu-id="ab21c-160">Implementuje ukázku:</span><span class="sxs-lookup"><span data-stu-id="ab21c-160">The sample implements:</span></span>

* <span data-ttu-id="ab21c-161">Základní spuštění tokenu monitorování.</span><span class="sxs-lookup"><span data-stu-id="ab21c-161">Basic startup token monitoring.</span></span>
* <span data-ttu-id="ab21c-162">Monitorování jako služba.</span><span class="sxs-lookup"><span data-stu-id="ab21c-162">Monitoring as a service.</span></span>
* <span data-ttu-id="ab21c-163">Mechanismus pro povolení a zákaz monitorování.</span><span class="sxs-lookup"><span data-stu-id="ab21c-163">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="ab21c-164">Vytvoří vzorovou `IConfigurationMonitor` rozhraní (*Extensions/ConfigurationMonitor.cs*):</span><span class="sxs-lookup"><span data-stu-id="ab21c-164">The sample establishes an `IConfigurationMonitor` interface (*Extensions/ConfigurationMonitor.cs*):</span></span>

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="ab21c-165">Konstruktor implementované třídy `ConfigurationMonitor`, zaregistruje zpětné volání upozornění na změny:</span><span class="sxs-lookup"><span data-stu-id="ab21c-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="ab21c-166">`config.GetReloadToken()`poskytuje token.</span><span class="sxs-lookup"><span data-stu-id="ab21c-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="ab21c-167">`InvokeChanged`je metoda zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="ab21c-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="ab21c-168">`state` v této instanci je řetězec, který popisuje monitorování stavu.</span><span class="sxs-lookup"><span data-stu-id="ab21c-168">The `state` in this instance is a string that describes the monitoring state.</span></span> <span data-ttu-id="ab21c-169">Dvě vlastnosti se používají:</span><span class="sxs-lookup"><span data-stu-id="ab21c-169">Two properties are used:</span></span>

* <span data-ttu-id="ab21c-170">`MonitoringEnabled`Určuje, pokud zpětné volání se budou spouštět jeho vlastní kód.</span><span class="sxs-lookup"><span data-stu-id="ab21c-170">`MonitoringEnabled` indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="ab21c-171">`CurrentState`Popisuje aktuální monitorování stavu pro použití v uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ab21c-171">`CurrentState` describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="ab21c-172">`InvokeChanged` Metoda je podobná starší přístup, s výjimkou, že:</span><span class="sxs-lookup"><span data-stu-id="ab21c-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="ab21c-173">Nefunguje jeho kód, pokud `MonitoringEnabled` je `true`.</span><span class="sxs-lookup"><span data-stu-id="ab21c-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="ab21c-174">Nastaví `CurrentState` vlastnost řetězec, který má popisný zprávu, která zaznamenává dobu, která byla spuštěna kód.</span><span class="sxs-lookup"><span data-stu-id="ab21c-174">Sets the `CurrentState` property string to a descriptive message that records the time that the code ran.</span></span>
* <span data-ttu-id="ab21c-175">Poznámky k aktuální `state` v jeho `WriteConsole` výstup.</span><span class="sxs-lookup"><span data-stu-id="ab21c-175">Notes the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="ab21c-176">Instance `ConfigurationMonitor` je zaregistrován jako služba v `ConfigureServices` z *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ab21c-176">An instance `ConfigurationMonitor` is registered as a service in `ConfigureServices` of *Startup.cs*:</span></span>

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="ab21c-177">Indexovou stránku nabízí uživatelského ovládacího prvku přes monitorování konfigurací.</span><span class="sxs-lookup"><span data-stu-id="ab21c-177">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="ab21c-178">Instance `IConfigurationMonitor` je vloženy do `IndexModel`:</span><span class="sxs-lookup"><span data-stu-id="ab21c-178">The instance of `IConfigurationMonitor` is injected into the `IndexModel`:</span></span>

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="ab21c-179">Tlačítko povolí nebo zakáže monitorování:</span><span class="sxs-lookup"><span data-stu-id="ab21c-179">A button enables and disables monitoring:</span></span>

[!code-cshtml[Main](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="ab21c-180">Když `OnPostStartMonitoring` je aktivována, je zapnuto monitorování a aktuální stav je vymazán.</span><span class="sxs-lookup"><span data-stu-id="ab21c-180">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="ab21c-181">Když `OnPostStopMonitoring` je aktivována, monitorování je zakázáno a stav nastaven tak, aby odrážela, že monitorování neprobíhá.</span><span class="sxs-lookup"><span data-stu-id="ab21c-181">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

## <a name="monitoring-cached-file-changes"></a><span data-ttu-id="ab21c-182">Monitorování změn souborů uložených v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="ab21c-182">Monitoring cached file changes</span></span>

<span data-ttu-id="ab21c-183">Obsah souboru může být uložená v mezipaměti v paměti pomocí [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span><span class="sxs-lookup"><span data-stu-id="ab21c-183">File content can be cached in-memory using [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span></span> <span data-ttu-id="ab21c-184">Ukládání do mezipaměti v paměti je podrobněji popsaná [ukládání do mezipaměti v paměti](xref:performance/caching/memory) tématu.</span><span class="sxs-lookup"><span data-stu-id="ab21c-184">In-memory caching is described in the [In-memory caching](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="ab21c-185">Bez nutnosti převádět další kroky, jako je popsáno níže, implementace *zastaralé* (zastaralé) data jsou vrácena z mezipaměti, pokud se změní zdrojová data.</span><span class="sxs-lookup"><span data-stu-id="ab21c-185">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="ab21c-186">Bez zohlednění stav v mezipaměti zdrojového souboru při obnovování [klouzavé vypršení platnosti](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) období vede k zastaralých data do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="ab21c-186">Not taking into account the status of a cached source file when renewing a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period leads to stale cache data.</span></span> <span data-ttu-id="ab21c-187">Každý požadavek pro data obnovuje klouzavou dobu vypršení platnosti, ale soubor nikdy znovu načíst do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="ab21c-187">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="ab21c-188">Všechny funkce aplikací, které používají obsah uložený v mezipaměti v souboru se vztahují pravděpodobně přijetí starý obsah.</span><span class="sxs-lookup"><span data-stu-id="ab21c-188">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="ab21c-189">Používání tokenů změn v souboru ukládání do mezipaměti scénář zabraňuje zastaralých souborů obsahu v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="ab21c-189">Using change tokens in a file caching scenario prevents stale file content in the cache.</span></span> <span data-ttu-id="ab21c-190">Ukázková aplikace ukazuje implementaci tohoto přístupu.</span><span class="sxs-lookup"><span data-stu-id="ab21c-190">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="ab21c-191">Příklad používá `GetFileContent` na:</span><span class="sxs-lookup"><span data-stu-id="ab21c-191">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="ab21c-192">Vrátí obsah souboru.</span><span class="sxs-lookup"><span data-stu-id="ab21c-192">Return file content.</span></span>
* <span data-ttu-id="ab21c-193">Implementujte algoritmus zkuste to znovu s exponenciální back vypnout pro případy, kdy uzamčení souboru dočasně znemožňuje soubor při čtení.</span><span class="sxs-lookup"><span data-stu-id="ab21c-193">Implement a retry algorithm with exponential back-off to cover cases where a file lock is temporarily preventing a file from being read.</span></span>

<span data-ttu-id="ab21c-194">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="ab21c-194">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="ab21c-195">A `FileService` se vytvoří pro zpracování vyhledávání v mezipaměti souborů.</span><span class="sxs-lookup"><span data-stu-id="ab21c-195">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="ab21c-196">`GetFileContent` Volání metody služby se pokusí získat obsah souboru z mezipaměti v paměti a obnoví v něm volajícímu (*Services/FileService.cs*).</span><span class="sxs-lookup"><span data-stu-id="ab21c-196">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="ab21c-197">Pokud obsah uložený v mezipaměti není nalezen pomocí klíče mezipaměti, budou provedeny následující akce:</span><span class="sxs-lookup"><span data-stu-id="ab21c-197">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="ab21c-198">Obsah souboru se získávají pomocí `GetFileContent`.</span><span class="sxs-lookup"><span data-stu-id="ab21c-198">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="ab21c-199">Změna token se získávají z poskytovatele souborů s [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span><span class="sxs-lookup"><span data-stu-id="ab21c-199">A change token is obtained from the file provider with [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span></span> <span data-ttu-id="ab21c-200">Zpětné volání je token se aktivuje, když je změny souboru.</span><span class="sxs-lookup"><span data-stu-id="ab21c-200">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="ab21c-201">Obsah souboru je uložené v mezipaměti s [klouzavé vypršení platnosti](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) období.</span><span class="sxs-lookup"><span data-stu-id="ab21c-201">The file content is cached with a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period.</span></span> <span data-ttu-id="ab21c-202">Token změny je připojené k [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) vyřazení položky mezipaměti, pokud se soubor změní, když se uloží do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="ab21c-202">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) to evict the cache entry if the file changes while it's cached.</span></span>

[!code-csharp[Main](change-tokens/sample/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="ab21c-203">`FileService` Je zaregistrován v kontejneru služby společně s paměti ukládání do mezipaměti služby (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="ab21c-203">The `FileService` is registered in the service container along with the memory caching service (*Startup.cs*):</span></span>

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet4)]

<span data-ttu-id="ab21c-204">Model stránka načte obsah souboru pomocí služby (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="ab21c-204">The page model loads the file's content using the service (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="ab21c-205">CompositeChangeToken – třída</span><span class="sxs-lookup"><span data-stu-id="ab21c-205">CompositeChangeToken class</span></span>

<span data-ttu-id="ab21c-206">Pro představující jednu nebo více `IChangeToken` instancí v jednom objektu, použijte [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) – třída ([odkaz na zdroj](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs)).</span><span class="sxs-lookup"><span data-stu-id="ab21c-206">For representing one or more `IChangeToken` instances in a single object, use the [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) class ([reference source](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs)).</span></span>

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

<span data-ttu-id="ab21c-207">`HasChanged`na složené tokenu sestavy `true` Pokud žádné reprezentované token `HasChanged` je `true`.</span><span class="sxs-lookup"><span data-stu-id="ab21c-207">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="ab21c-208">`ActiveChangeCallbacks`na složené tokenu sestavy `true` Pokud žádné reprezentované token `ActiveChangeCallbacks` je `true`.</span><span class="sxs-lookup"><span data-stu-id="ab21c-208">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="ab21c-209">Pokud dojde k události více souběžných změny, je vyvolána zpětného volání kompozitních změn přesně jednou.</span><span class="sxs-lookup"><span data-stu-id="ab21c-209">If multiple concurrent change events occur, the composite change callback is invoked exactly one time.</span></span>

## <a name="see-also"></a><span data-ttu-id="ab21c-210">Viz také</span><span class="sxs-lookup"><span data-stu-id="ab21c-210">See also</span></span>

* [<span data-ttu-id="ab21c-211">Ukládání do mezipaměti webového serveru</span><span class="sxs-lookup"><span data-stu-id="ab21c-211">In-memory caching</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="ab21c-212">Práce s distribuované mezipaměti</span><span class="sxs-lookup"><span data-stu-id="ab21c-212">Working with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="ab21c-213">Detekovat změny s tokeny změn</span><span class="sxs-lookup"><span data-stu-id="ab21c-213">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="ab21c-214">Ukládání odpovědí do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="ab21c-214">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="ab21c-215">Middleware pro ukládání odpovědí do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="ab21c-215">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="ab21c-216">Uložení pomocné rutiny značky do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="ab21c-216">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="ab21c-217">Pomocná rutina značek v distribuované mezipaměti</span><span class="sxs-lookup"><span data-stu-id="ab21c-217">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
