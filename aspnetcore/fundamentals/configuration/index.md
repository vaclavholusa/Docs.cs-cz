---
title: Konfigurace v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak použít rozhraní API pro konfiguraci ke konfiguraci aplikace ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 59ab0cd0f6975d15bd01ce7e4128521938182c24
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228621"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="128b3-103">Konfigurace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="128b3-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="128b3-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [označit Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), a [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="128b3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="128b3-105">Konfigurační rozhraní API poskytuje způsob, jak nakonfigurovat v ASP.NET Core webové aplikace založené na seznam dvojic název hodnota.</span><span class="sxs-lookup"><span data-stu-id="128b3-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="128b3-106">Konfigurace je pro čtení v době běhu z více zdrojů.</span><span class="sxs-lookup"><span data-stu-id="128b3-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="128b3-107">Dvojice název hodnota mohou být seskupeny do více úrovní hierarchie.</span><span class="sxs-lookup"><span data-stu-id="128b3-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="128b3-108">Existují zprostředkovatele konfigurace pro:</span><span class="sxs-lookup"><span data-stu-id="128b3-108">There are configuration providers for:</span></span>

* <span data-ttu-id="128b3-109">Formáty souborů (INI, JSON a XML).</span><span class="sxs-lookup"><span data-stu-id="128b3-109">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="128b3-110">Argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="128b3-110">Command-line arguments.</span></span>
* <span data-ttu-id="128b3-111">Proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="128b3-111">Environment variables.</span></span>
* <span data-ttu-id="128b3-112">Objekty .NET v paměti.</span><span class="sxs-lookup"><span data-stu-id="128b3-112">In-memory .NET objects.</span></span>
* <span data-ttu-id="128b3-113">Nešifrované [manažera tajných](xref:security/app-secrets) úložiště.</span><span class="sxs-lookup"><span data-stu-id="128b3-113">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="128b3-114">Uložení šifrovaného uživatelského, jako například [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="128b3-114">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="128b3-115">Vlastní zprostředkovatelé (nainstalované nebo vytváření).</span><span class="sxs-lookup"><span data-stu-id="128b3-115">Custom providers (installed or created).</span></span>

<span data-ttu-id="128b3-116">Každá hodnota konfigurace mapuje na řetězec klíče.</span><span class="sxs-lookup"><span data-stu-id="128b3-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="128b3-117">Je dostupná podpora integrované vazbu k deserializaci nastavení do vlastní [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objektu (jednoduchá třída .NET s vlastnostmi).</span><span class="sxs-lookup"><span data-stu-id="128b3-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="128b3-118">Možnosti vzor používá k reprezentování skupiny související nastavení možnosti třídy.</span><span class="sxs-lookup"><span data-stu-id="128b3-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="128b3-119">Další informace o použití vzoru možnosti najdete v tématu [možnosti](xref:fundamentals/configuration/options) tématu.</span><span class="sxs-lookup"><span data-stu-id="128b3-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="128b3-120">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="128b3-120">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="128b3-121">Příklady uvedené v tomto tématu využívají:</span><span class="sxs-lookup"><span data-stu-id="128b3-121">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="128b3-122">Nastavení základní cesty aplikace s využitím [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span><span class="sxs-lookup"><span data-stu-id="128b3-122">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="128b3-123">`SetBasePath` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="128b3-123">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="128b3-124">Řešení oddíly konfiguračních souborů s [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span><span class="sxs-lookup"><span data-stu-id="128b3-124">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="128b3-125">`GetSection` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="128b3-125">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="128b3-126">Konfigurace vazby s [svázat](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span><span class="sxs-lookup"><span data-stu-id="128b3-126">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="128b3-127">`Bind` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="128b3-127">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

<span data-ttu-id="128b3-128">Tyto balíčky jsou součástí [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="128b3-128">These packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="128b3-129">Příklady uvedené v tomto tématu využívají:</span><span class="sxs-lookup"><span data-stu-id="128b3-129">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="128b3-130">Nastavení základní cesty aplikace s využitím [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span><span class="sxs-lookup"><span data-stu-id="128b3-130">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="128b3-131">`SetBasePath` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="128b3-131">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="128b3-132">Řešení oddíly konfiguračních souborů s [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span><span class="sxs-lookup"><span data-stu-id="128b3-132">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="128b3-133">`GetSection` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="128b3-133">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="128b3-134">Konfigurace vazby s [svázat](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span><span class="sxs-lookup"><span data-stu-id="128b3-134">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="128b3-135">`Bind` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="128b3-135">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

<span data-ttu-id="128b3-136">Tyto balíčky jsou součástí [metabalíček Microsoft.aspnetcore.all](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="128b3-136">These packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="128b3-137">Příklady uvedené v tomto tématu využívají:</span><span class="sxs-lookup"><span data-stu-id="128b3-137">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="128b3-138">Nastavení základní cesty aplikace s využitím [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span><span class="sxs-lookup"><span data-stu-id="128b3-138">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="128b3-139">`SetBasePath` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="128b3-139">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="128b3-140">Řešení oddíly konfiguračních souborů s [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span><span class="sxs-lookup"><span data-stu-id="128b3-140">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="128b3-141">`GetSection` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="128b3-141">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="128b3-142">Konfigurace vazby s [svázat](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span><span class="sxs-lookup"><span data-stu-id="128b3-142">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="128b3-143">`Bind` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="128b3-143">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

::: moniker-end

## <a name="json-configuration"></a><span data-ttu-id="128b3-144">Konfigurace JSON</span><span class="sxs-lookup"><span data-stu-id="128b3-144">JSON configuration</span></span>

<span data-ttu-id="128b3-145">Následující aplikace konzoly používá zprostředkovatel konfigurace JSON:</span><span class="sxs-lookup"><span data-stu-id="128b3-145">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="128b3-146">Aplikace načte a zobrazí následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="128b3-146">The app reads and displays the following configuration settings:</span></span>

[!code-json[](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="128b3-147">Konfigurace se skládá z hierarchický seznam dvojic název hodnota, ve kterých uzlech jsou odděleny dvojtečkou (`:`).</span><span class="sxs-lookup"><span data-stu-id="128b3-147">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon (`:`).</span></span> <span data-ttu-id="128b3-148">Pokud chcete načíst hodnotu, přístup k `Configuration` indexer s klíčem odpovídající položka:</span><span class="sxs-lookup"><span data-stu-id="128b3-148">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="128b3-149">Pro práci s poli v konfiguraci ve formátu JSON zdrojích, použijte jako součást řetězců oddělených indexu pole.</span><span class="sxs-lookup"><span data-stu-id="128b3-149">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="128b3-150">Následující příklad získá název první položky v předchozím `wizards` pole:</span><span class="sxs-lookup"><span data-stu-id="128b3-150">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="128b3-151">Dvojice název hodnota zapsána do integrovaného [konfigurace](/dotnet/api/microsoft.extensions.configuration) zprostředkovatelé jsou **není** trvalé.</span><span class="sxs-lookup"><span data-stu-id="128b3-151">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="128b3-152">Ale je možné vytvořit vlastní poskytovatele, který uloží hodnoty.</span><span class="sxs-lookup"><span data-stu-id="128b3-152">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="128b3-153">Zobrazit [vlastního poskytovatele konfigurace](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="128b3-153">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="128b3-154">V předchozím příkladu používá ke čtení hodnot konfigurace indexeru.</span><span class="sxs-lookup"><span data-stu-id="128b3-154">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="128b3-155">Získat přístup ke konfiguraci mimo `Startup`, použijte *možnosti vzor*.</span><span class="sxs-lookup"><span data-stu-id="128b3-155">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="128b3-156">Další informace najdete v tématu [možnosti](xref:fundamentals/configuration/options) tématu.</span><span class="sxs-lookup"><span data-stu-id="128b3-156">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="128b3-157">Konfiguraci XML</span><span class="sxs-lookup"><span data-stu-id="128b3-157">XML configuration</span></span>

<span data-ttu-id="128b3-158">Pro práci s poli v konfiguraci ve formátu XML zdroje poskytují `name` index na každý prvek.</span><span class="sxs-lookup"><span data-stu-id="128b3-158">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="128b3-159">Použití indexu pro přístup k hodnotám:</span><span class="sxs-lookup"><span data-stu-id="128b3-159">Use the index to access the values:</span></span>

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a><span data-ttu-id="128b3-160">Konfigurace podle prostředí</span><span class="sxs-lookup"><span data-stu-id="128b3-160">Configuration by environment</span></span>

<span data-ttu-id="128b3-161">Je obvykle mají různá nastavení konfigurace pro různá prostředí, třeba vývoj, testování a produkce.</span><span class="sxs-lookup"><span data-stu-id="128b3-161">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="128b3-162">`CreateDefaultBuilder` Metody rozšíření v aplikaci ASP.NET Core 2.x (nebo pomocí `AddJsonFile` a `AddEnvironmentVariables` přímo v aplikaci ASP.NET Core 1.x) přidá poskytovatele konfigurace pro čtení souborů JSON a systém konfigurace zdroje:</span><span class="sxs-lookup"><span data-stu-id="128b3-162">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="128b3-163">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="128b3-163">*appsettings.json*</span></span>
* <span data-ttu-id="128b3-164">*appsettings.\<EnvironmentName>.json*</span><span class="sxs-lookup"><span data-stu-id="128b3-164">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="128b3-165">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="128b3-165">Environment variables</span></span>

<span data-ttu-id="128b3-166">Aplikace ASP.NET Core 1.x potřebné k volání `AddJsonFile` a [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="128b3-166">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="128b3-167">Zobrazit [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) vysvětlení parametrů.</span><span class="sxs-lookup"><span data-stu-id="128b3-167">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="128b3-168">`reloadOnChange` je podporován pouze v ASP.NET Core 1.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="128b3-168">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="128b3-169">Konfigurace zdrojů se čtení v pořadí, zda jste zadali.</span><span class="sxs-lookup"><span data-stu-id="128b3-169">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="128b3-170">V předchozím kódu jsou proměnné prostředí načteny poslední.</span><span class="sxs-lookup"><span data-stu-id="128b3-170">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="128b3-171">Všechny hodnoty konfigurace nastavit pomocí prostředí nahradit v dva poskytovatelé předchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="128b3-171">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="128b3-172">Vezměte v úvahu následující *appsettings. Staging.JSON* souboru:</span><span class="sxs-lookup"><span data-stu-id="128b3-172">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[](index/sample/appsettings.Staging.json)]

<span data-ttu-id="128b3-173">V následujícím kódu `Configure` přečte hodnotu `MyConfig`:</span><span class="sxs-lookup"><span data-stu-id="128b3-173">In the following code, `Configure` reads the value of `MyConfig`:</span></span>

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

<span data-ttu-id="128b3-174">Prostředí se obvykle nastavuje na `Development`, `Staging`, nebo `Production`.</span><span class="sxs-lookup"><span data-stu-id="128b3-174">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="128b3-175">Další informace najdete v tématu [používání více prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="128b3-175">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="128b3-176">Požadavky na konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="128b3-176">Configuration considerations:</span></span>

* <span data-ttu-id="128b3-177">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) znovu načíst konfigurační data při změně.</span><span class="sxs-lookup"><span data-stu-id="128b3-177">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) can reload configuration data when it changes.</span></span>
* <span data-ttu-id="128b3-178">Konfigurační klíče jsou **není** malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="128b3-178">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="128b3-179">**Nikdy** ukládat hesla a jiná citlivá data v konfiguraci zprostředkovatele kódu nebo v konfiguračních souborech na prostý text.</span><span class="sxs-lookup"><span data-stu-id="128b3-179">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="128b3-180">Nechcete používat produkční tajných kódů při vývoji nebo testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="128b3-180">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="128b3-181">Zadejte tajných kódů mimo projekt tak, že nemohou být omylem zaměřuje na úložiště zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="128b3-181">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="128b3-182">Další informace o [jak používat více prostředí](xref:fundamentals/environments) a správu [bezpečné ukládání tajných kódů aplikace při vývoji](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="128b3-182">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets in development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="128b3-183">Hierarchické konfigurační hodnoty zadané v proměnné prostředí, dvojtečkou (`:`) nemusí fungovat na všech platformách.</span><span class="sxs-lookup"><span data-stu-id="128b3-183">For hierarchical config values specified in environment variables, a colon (`:`) may not work on all platforms.</span></span> <span data-ttu-id="128b3-184">Dvojitým podtržítkem (`__`) podporuje všechny platformy.</span><span class="sxs-lookup"><span data-stu-id="128b3-184">Double underscore (`__`) is supported by all platforms.</span></span>
* <span data-ttu-id="128b3-185">Při interakci s konfigurací rozhraní API, dvojtečkou (`:`) funguje na všech platformách.</span><span class="sxs-lookup"><span data-stu-id="128b3-185">When interacting with the configuration API, a colon (`:`) works on all platforms.</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="128b3-186">Zprostředkovatel v paměti a vytvoření vazby na třídu POCO</span><span class="sxs-lookup"><span data-stu-id="128b3-186">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="128b3-187">Následující příklad ukazuje způsob použití poskytovatele v paměti a vytvořit vazbu na třídu:</span><span class="sxs-lookup"><span data-stu-id="128b3-187">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[](index/sample/InMemory/Program.cs)]

<span data-ttu-id="128b3-188">Hodnoty konfigurace se vrátí jako řetězce, ale vazba umožňuje konstrukce objektů.</span><span class="sxs-lookup"><span data-stu-id="128b3-188">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="128b3-189">Vazba umožňuje načtení objektů POCO nebo dokonce celý objekt grafu.</span><span class="sxs-lookup"><span data-stu-id="128b3-189">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="128b3-190">GetValue</span><span class="sxs-lookup"><span data-stu-id="128b3-190">GetValue</span></span>

<span data-ttu-id="128b3-191">Následující příklad ukazuje, [GetValue&lt;T&gt; ](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) – metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="128b3-191">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="128b3-192">ConfigurationBinder `GetValue<T>` metoda umožňuje specifikace výchozí hodnotu (80 v ukázce).</span><span class="sxs-lookup"><span data-stu-id="128b3-192">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="128b3-193">`GetValue<T>` bez vazby na celé části je pro jednoduché scénáře.</span><span class="sxs-lookup"><span data-stu-id="128b3-193">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="128b3-194">`GetValue<T>` Získá skalárních hodnot z `GetSection(key).Value` převedeny na určitý typ.</span><span class="sxs-lookup"><span data-stu-id="128b3-194">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="128b3-195">Vytvořit vazbu grafu objektu</span><span class="sxs-lookup"><span data-stu-id="128b3-195">Bind to an object graph</span></span>

<span data-ttu-id="128b3-196">Každý objekt ve třídě může být vázaný rekurzivně.</span><span class="sxs-lookup"><span data-stu-id="128b3-196">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="128b3-197">Vezměte v úvahu následující `AppSettings` třídy:</span><span class="sxs-lookup"><span data-stu-id="128b3-197">Consider the following `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="128b3-198">Následující příklad vytvoří vazbu `AppSettings` třídy:</span><span class="sxs-lookup"><span data-stu-id="128b3-198">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="128b3-199">**ASP.NET Core 1.1** a vyšší můžete použít `Get<T>`, který pracuje s celé oddíly.</span><span class="sxs-lookup"><span data-stu-id="128b3-199">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="128b3-200">`Get<T>` může být výhodnější než použití `Bind`.</span><span class="sxs-lookup"><span data-stu-id="128b3-200">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="128b3-201">Následující kód ukazuje, jak používat `Get<T>` s předchozím příkladu:</span><span class="sxs-lookup"><span data-stu-id="128b3-201">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="128b3-202">Pomocí následujících *appsettings.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="128b3-202">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="128b3-203">Program zobrazí `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="128b3-203">The program displays `Height 11`.</span></span>

<span data-ttu-id="128b3-204">Následující kód slouží k jednotce pro test konfigurace:</span><span class="sxs-lookup"><span data-stu-id="128b3-204">The following code can be used to unit test the configuration:</span></span>

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="128b3-205">Vytvoření vlastního zprostředkovatele Entity Framework</span><span class="sxs-lookup"><span data-stu-id="128b3-205">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="128b3-206">V této části se vytvoří zprostředkovatel základní konfigurace, který přečte dvojice název hodnota v databázi pomocí EF.</span><span class="sxs-lookup"><span data-stu-id="128b3-206">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="128b3-207">Definování `ConfigurationValue` entity pro ukládání hodnoty konfigurace v databázi:</span><span class="sxs-lookup"><span data-stu-id="128b3-207">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="128b3-208">Přidat `ConfigurationContext` ukládání a přístup k nakonfigurované hodnoty:</span><span class="sxs-lookup"><span data-stu-id="128b3-208">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="128b3-209">Vytvořte třídu, která implementuje [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span><span class="sxs-lookup"><span data-stu-id="128b3-209">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="128b3-210">Vytvoření vlastního poskytovatele konfigurace děděním z [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="128b3-210">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="128b3-211">Poskytovatel konfigurace inicializuje databáze, pokud je prázdný:</span><span class="sxs-lookup"><span data-stu-id="128b3-211">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="128b3-212">Zvýrazněné hodnoty z databáze ("value_from_ef_1" a "value_from_ef_2") se zobrazí při spuštění ukázky.</span><span class="sxs-lookup"><span data-stu-id="128b3-212">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="128b3-213">`EFConfigSource` Lze použít rozšiřující metodu pro přidání zdroj konfigurace:</span><span class="sxs-lookup"><span data-stu-id="128b3-213">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="128b3-214">Následující kód ukazuje, jak použít vlastní `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="128b3-214">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="128b3-215">Poznámka: Ukázka přidá vlastní `EFConfigProvider` po poskytovatele formátu JSON, takže všechna nastavení z databáze přepíší nastavení z *appsettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="128b3-215">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="128b3-216">Pomocí následujících *appsettings.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="128b3-216">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="128b3-217">Zobrazí se následující výstup:</span><span class="sxs-lookup"><span data-stu-id="128b3-217">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="128b3-218">Zprostředkovatel konfigurace příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="128b3-218">CommandLine configuration provider</span></span>

<span data-ttu-id="128b3-219">[Poskytovatele konfigurace CommandLine](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) přijímá argument příkazového řádku páry klíč hodnota pro konfiguraci za běhu.</span><span class="sxs-lookup"><span data-stu-id="128b3-219">The [CommandLine configuration provider](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="128b3-220">Zobrazení nebo stažení ukázky konfigurace příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="128b3-220">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="128b3-221">Nastavit a používat ho zprostředkovatel konfigurace příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="128b3-221">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="128b3-222">Základní konfigurace</span><span class="sxs-lookup"><span data-stu-id="128b3-222">Basic Configuration</span></span>](#tab/basicconfiguration/)

<span data-ttu-id="128b3-223">Chcete-li aktivovat příkazového řádku konfigurace, zavolejte `AddCommandLine` rozšiřující metody na instanci [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="128b3-223">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="128b3-224">Spuštění kódu se zobrazí následující výstup:</span><span class="sxs-lookup"><span data-stu-id="128b3-224">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="128b3-225">Předávání argumentů páry klíč hodnota v příkazovém řádku změní hodnoty `Profile:MachineName` a `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="128b3-225">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="128b3-226">V okně konzoly se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="128b3-226">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="128b3-227">Chcete-li přepsat konfiguraci poskytované ostatní poskytovatelé konfigurace pomocí příkazového řádku konfigurace, zavolejte `AddCommandLine` posledního `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="128b3-227">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="128b3-228">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="128b3-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="128b3-229">Typická aplikace ASP.NET Core 2.x použít metodu statické pohodlí `CreateDefaultBuilder` k sestavení hostitele:</span><span class="sxs-lookup"><span data-stu-id="128b3-229">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="128b3-230">`CreateDefaultBuilder` načte volitelné konfigurace z *appsettings.json*, *appsettings. { Prostředí} .json*, [tajných kódů uživatelů](xref:security/app-secrets) (v `Development` prostředí), proměnné a argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="128b3-230">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="128b3-231">Zprostředkovatel konfigurace příkazového řádku se nazývá poslední.</span><span class="sxs-lookup"><span data-stu-id="128b3-231">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="128b3-232">Poslední volání zprostředkovatele umožňuje argumenty příkazového řádku předané v době běhu k přepsání konfigurace nastavená ostatní poskytovatelé konfigurace volat dříve.</span><span class="sxs-lookup"><span data-stu-id="128b3-232">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="128b3-233">Pro *appsettings* soubory, kde:</span><span class="sxs-lookup"><span data-stu-id="128b3-233">For *appsettings* files where:</span></span>

* <span data-ttu-id="128b3-234">`reloadOnChange` je povolená.</span><span class="sxs-lookup"><span data-stu-id="128b3-234">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="128b3-235">Obsahují stejné nastavení argumentů příkazového řádku a *appsettings* souboru.</span><span class="sxs-lookup"><span data-stu-id="128b3-235">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="128b3-236">*Appsettings* změní soubor, který obsahuje odpovídající argument příkazového řádku po spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="128b3-236">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="128b3-237">Pokud jsou splněné všechny výše uvedené podmínky, argumenty příkazového řádku se přepíšou.</span><span class="sxs-lookup"><span data-stu-id="128b3-237">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="128b3-238">Můžete použít aplikace ASP.NET Core 2.x [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) místo `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="128b3-238">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="128b3-239">Při použití `WebHostBuilder`, je nutné ručně nastavit konfiguraci s [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="128b3-239">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="128b3-240">Zobrazit na kartě ASP.NET Core 1.x pro další informace.</span><span class="sxs-lookup"><span data-stu-id="128b3-240">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="128b3-241">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="128b3-241">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="128b3-242">Vytvoření [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) a volat `AddCommandLine` metodu použít poskytovatele konfigurace příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="128b3-242">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="128b3-243">Poslední volání zprostředkovatele umožňuje argumenty příkazového řádku předané v době běhu k přepsání konfigurace nastavená ostatní poskytovatelé konfigurace volat dříve.</span><span class="sxs-lookup"><span data-stu-id="128b3-243">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="128b3-244">Použít konfiguraci [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) s `UseConfiguration` metody:</span><span class="sxs-lookup"><span data-stu-id="128b3-244">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="128b3-245">Arguments</span><span class="sxs-lookup"><span data-stu-id="128b3-245">Arguments</span></span>

<span data-ttu-id="128b3-246">Argumenty předané v příkazovém řádku musí odpovídat jednomu ze dvou formátů je znázorněno v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="128b3-246">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="128b3-247">Argument formátu</span><span class="sxs-lookup"><span data-stu-id="128b3-247">Argument format</span></span>                                                     | <span data-ttu-id="128b3-248">Příklad</span><span class="sxs-lookup"><span data-stu-id="128b3-248">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="128b3-249">Jeden argument: pár klíč hodnota oddělené symbolem rovná (`=`)</span><span class="sxs-lookup"><span data-stu-id="128b3-249">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="128b3-250">Posloupnost dva argumenty: pár klíč hodnota oddělené mezerou</span><span class="sxs-lookup"><span data-stu-id="128b3-250">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="128b3-251">**Jediný argument**</span><span class="sxs-lookup"><span data-stu-id="128b3-251">**Single argument**</span></span>

<span data-ttu-id="128b3-252">Hodnota musí následovat znak rovná se (`=`).</span><span class="sxs-lookup"><span data-stu-id="128b3-252">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="128b3-253">Hodnota může mít hodnotu null (například `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="128b3-253">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="128b3-254">Klíč může mít předponu.</span><span class="sxs-lookup"><span data-stu-id="128b3-254">The key may have a prefix.</span></span>

| <span data-ttu-id="128b3-255">Klíčová předpona</span><span class="sxs-lookup"><span data-stu-id="128b3-255">Key prefix</span></span>               | <span data-ttu-id="128b3-256">Příklad</span><span class="sxs-lookup"><span data-stu-id="128b3-256">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="128b3-257">Žádná předpona.</span><span class="sxs-lookup"><span data-stu-id="128b3-257">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="128b3-258">Jeden pomlčka (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="128b3-258">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="128b3-259">Dvě pomlčky (`--`)</span><span class="sxs-lookup"><span data-stu-id="128b3-259">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="128b3-260">Lomítko (`/`)</span><span class="sxs-lookup"><span data-stu-id="128b3-260">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="128b3-261">&#8224;Klíč s předponou jediného (`-`) musí být zadaná ve [přepnout mapování](#switch-mappings), které jsou popsány níže.</span><span class="sxs-lookup"><span data-stu-id="128b3-261">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="128b3-262">Příklad příkazu:</span><span class="sxs-lookup"><span data-stu-id="128b3-262">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="128b3-263">Poznámka: Pokud `-key2` není k dispozici v [přepnout mapování](#switch-mappings) předaná zprostředkovateli konfigurace `FormatException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="128b3-263">Note: If `-key2` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="128b3-264">**Pořadí dvou argumentů**</span><span class="sxs-lookup"><span data-stu-id="128b3-264">**Sequence of two arguments**</span></span>

<span data-ttu-id="128b3-265">Hodnota nemůže mít hodnotu null a musí dodržovat klíč oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="128b3-265">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="128b3-266">Klíč musí mít předponu.</span><span class="sxs-lookup"><span data-stu-id="128b3-266">The key must have a prefix.</span></span>

| <span data-ttu-id="128b3-267">Klíčová předpona</span><span class="sxs-lookup"><span data-stu-id="128b3-267">Key prefix</span></span>               | <span data-ttu-id="128b3-268">Příklad</span><span class="sxs-lookup"><span data-stu-id="128b3-268">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="128b3-269">Jeden pomlčka (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="128b3-269">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="128b3-270">Dvě pomlčky (`--`)</span><span class="sxs-lookup"><span data-stu-id="128b3-270">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="128b3-271">Lomítko (`/`)</span><span class="sxs-lookup"><span data-stu-id="128b3-271">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="128b3-272">&#8224;Klíč s předponou jediného (`-`) musí být zadaná ve [přepnout mapování](#switch-mappings), které jsou popsány níže.</span><span class="sxs-lookup"><span data-stu-id="128b3-272">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="128b3-273">Příklad příkazu:</span><span class="sxs-lookup"><span data-stu-id="128b3-273">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="128b3-274">Poznámka: Pokud `-key1` není k dispozici v [přepnout mapování](#switch-mappings) předaná zprostředkovateli konfigurace `FormatException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="128b3-274">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="128b3-275">Duplicitní klíče</span><span class="sxs-lookup"><span data-stu-id="128b3-275">Duplicate keys</span></span>

<span data-ttu-id="128b3-276">Pokud duplicitní klíče jsou k dispozici, použije se poslední páru klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="128b3-276">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="128b3-277">Přepnout mapování</span><span class="sxs-lookup"><span data-stu-id="128b3-277">Switch mappings</span></span>

<span data-ttu-id="128b3-278">Při ruční sestavení konfigurace s `ConfigurationBuilder`, slovník mapování přepínače lze přidat do `AddCommandLine` metody.</span><span class="sxs-lookup"><span data-stu-id="128b3-278">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="128b3-279">Mapování přepínači povolit název klíče pro náhradní logiku.</span><span class="sxs-lookup"><span data-stu-id="128b3-279">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="128b3-280">Při použití přepínače slovníku mapování slovníku se kontroluje u klíč, který odpovídá key určeného tímto argumentem příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="128b3-280">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="128b3-281">Pokud je nalezen příkazového řádku klíč ve slovníku, hodnota slovníku (náhradní klíč) se předá zpět do nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="128b3-281">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="128b3-282">Mapování přepínač je vyžadován pro libovolného příkazového řádku klíče předponu jeden pomlčka (`-`).</span><span class="sxs-lookup"><span data-stu-id="128b3-282">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="128b3-283">Přepnout mapování slovníku klíčových pravidel:</span><span class="sxs-lookup"><span data-stu-id="128b3-283">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="128b3-284">Přepínače musí začínat pomlčkou (`-`) nebo dvojitou pomlčka (`--`).</span><span class="sxs-lookup"><span data-stu-id="128b3-284">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="128b3-285">Slovník mapování přepínač nesmí obsahovat duplicitní klíče.</span><span class="sxs-lookup"><span data-stu-id="128b3-285">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="128b3-286">V následujícím příkladu `GetSwitchMappings` metoda umožňuje používat jeden dash argumenty příkazového řádku (`-`) předpona klíče a vyhnout se počáteční podklíče předpony.</span><span class="sxs-lookup"><span data-stu-id="128b3-286">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="128b3-287">Bez zadání argumentů příkazového řádku, které slovník `AddInMemoryCollection` nastaví hodnoty konfigurace.</span><span class="sxs-lookup"><span data-stu-id="128b3-287">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="128b3-288">Spusťte aplikaci následujícím příkazem:</span><span class="sxs-lookup"><span data-stu-id="128b3-288">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="128b3-289">V okně konzoly se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="128b3-289">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="128b3-290">Použijte následující postupy k předávání nastavení konfigurace:</span><span class="sxs-lookup"><span data-stu-id="128b3-290">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="128b3-291">V okně konzoly se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="128b3-291">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="128b3-292">Po vytvoření slovníku mapování přepínač obsahuje data zobrazená v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="128b3-292">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="128b3-293">Key</span><span class="sxs-lookup"><span data-stu-id="128b3-293">Key</span></span>            | <span data-ttu-id="128b3-294">Hodnota</span><span class="sxs-lookup"><span data-stu-id="128b3-294">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="128b3-295">Abychom si předvedli klíče přepínání pomocí slovníku, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="128b3-295">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="128b3-296">Jsou přehozeny příkazového řádku klíče.</span><span class="sxs-lookup"><span data-stu-id="128b3-296">The command-line keys are swapped.</span></span> <span data-ttu-id="128b3-297">V okně konzoly se zobrazí konfigurační hodnoty pro `Profile:MachineName` a `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="128b3-297">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="128b3-298">soubor Web.config</span><span class="sxs-lookup"><span data-stu-id="128b3-298">web.config file</span></span>

<span data-ttu-id="128b3-299">A *web.config* soubor je vyžadován při hostování aplikace v IIS nebo IIS Express.</span><span class="sxs-lookup"><span data-stu-id="128b3-299">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="128b3-300">Nastavení v *web.config* povolit [modul ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) spusťte aplikaci a nakonfigurovat další nastavení služby IIS a modulů.</span><span class="sxs-lookup"><span data-stu-id="128b3-300">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="128b3-301">Pokud *web.config* není k dispozici soubor a soubor projektu obsahuje `<Project Sdk="Microsoft.NET.Sdk.Web">`, publikování projektu vytvoří *web.config* souborů v publikované výstupu ( *publikovat* složky).</span><span class="sxs-lookup"><span data-stu-id="128b3-301">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="128b3-302">Další informace najdete v tématu [hostitele ASP.NET Core ve Windows se službou IIS](xref:host-and-deploy/iis/index#webconfig-file).</span><span class="sxs-lookup"><span data-stu-id="128b3-302">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="access-configuration-during-startup"></a><span data-ttu-id="128b3-303">Konfigurace přístupu při spuštění</span><span class="sxs-lookup"><span data-stu-id="128b3-303">Access configuration during startup</span></span>

<span data-ttu-id="128b3-304">Získat přístup ke konfiguraci v rámci `ConfigureServices` nebo `Configure` během spuštění, podívejte se na příklady v [spuštění aplikace](xref:fundamentals/startup) tématu.</span><span class="sxs-lookup"><span data-stu-id="128b3-304">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="adding-configuration-from-an-external-assembly"></a><span data-ttu-id="128b3-305">Přidání konfigurace z externího sestavení</span><span class="sxs-lookup"><span data-stu-id="128b3-305">Adding configuration from an external assembly</span></span>

<span data-ttu-id="128b3-306">[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementace umožňuje přidání vylepšení do aplikace při spuštění z externího sestavení mimo aplikaci prvku `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="128b3-306">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="128b3-307">Další informace najdete v tématu [vylepšení aplikace z externího sestavení](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="128b3-307">For more information, see [Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a><span data-ttu-id="128b3-308">Konfigurace přístupu v zobrazení stránky Razor nebo MVC</span><span class="sxs-lookup"><span data-stu-id="128b3-308">Access configuration in a Razor Page or MVC view</span></span>

<span data-ttu-id="128b3-309">Chcete-li získat přístup k nastavení konfigurace v zobrazení MVC nebo stránky Razor Pages, přidejte [using – direktiva](xref:mvc/views/razor#using) ([referenční dokumentace jazyka C#: použití direktivy](/dotnet/csharp/language-reference/keywords/using-directive)) pro [Microsoft.Extensions.Configuration obor názvů ](/dotnet/api/microsoft.extensions.configuration) a vložit [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) do stránky nebo zobrazení.</span><span class="sxs-lookup"><span data-stu-id="128b3-309">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](/dotnet/api/microsoft.extensions.configuration) and inject [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) into the page or view.</span></span>

<span data-ttu-id="128b3-310">Na stránce pro stránky Razor:</span><span class="sxs-lookup"><span data-stu-id="128b3-310">In a Razor Pages page:</span></span>

```cshtml
@page
@model IndexModel

@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="128b3-311">V zobrazení MVC:</span><span class="sxs-lookup"><span data-stu-id="128b3-311">In an MVC view:</span></span>

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

## <a name="additional-notes"></a><span data-ttu-id="128b3-312">Další poznámky</span><span class="sxs-lookup"><span data-stu-id="128b3-312">Additional notes</span></span>

* <span data-ttu-id="128b3-313">Dependency Injection (DI) není nastavená nahoru, až po `ConfigureServices` je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="128b3-313">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="128b3-314">Konfigurace systému není DI vědět.</span><span class="sxs-lookup"><span data-stu-id="128b3-314">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="128b3-315">`IConfiguration` má dvě specializace:</span><span class="sxs-lookup"><span data-stu-id="128b3-315">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="128b3-316">`IConfigurationRoot` Používá se pro kořenový uzel.</span><span class="sxs-lookup"><span data-stu-id="128b3-316">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="128b3-317">Můžete aktivovat znova načíst.</span><span class="sxs-lookup"><span data-stu-id="128b3-317">Can trigger a reload.</span></span>
  * <span data-ttu-id="128b3-318">`IConfigurationSection` Reprezentuje oddíl konfigurační hodnoty.</span><span class="sxs-lookup"><span data-stu-id="128b3-318">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="128b3-319">`GetSection` a `GetChildren` metody vrátit `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="128b3-319">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="128b3-320">Použití [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) při opětovném načtení konfigurace nebo pro přístup do každého zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="128b3-320">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="128b3-321">Ani jeden z těchto situacích jsou běžné.</span><span class="sxs-lookup"><span data-stu-id="128b3-321">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="128b3-322">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="128b3-322">Additional resources</span></span>

* [<span data-ttu-id="128b3-323">Možnosti</span><span class="sxs-lookup"><span data-stu-id="128b3-323">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="128b3-324">Používání více prostředí</span><span class="sxs-lookup"><span data-stu-id="128b3-324">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="128b3-325">Bezpečné ukládání tajných kódů aplikace při vývoji</span><span class="sxs-lookup"><span data-stu-id="128b3-325">Safe storage of app secrets in development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="128b3-326">Hostitel v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="128b3-326">Host in ASP.NET Core</span></span>](xref:fundamentals/host/index)
* [<span data-ttu-id="128b3-327">Injektáž závislostí</span><span class="sxs-lookup"><span data-stu-id="128b3-327">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="128b3-328">Zprostředkovatel konfigurace služby Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="128b3-328">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
