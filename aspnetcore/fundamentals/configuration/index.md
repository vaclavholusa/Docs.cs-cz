---
title: Konfigurace v ASP.NET Core
author: rick-anderson
description: "Použijte rozhraní API konfigurace pro konfiguraci aplikace ASP.NET Core několik metod."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: 7c41621db835b452c9aad9463a9ffccdf0c06484
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="f883e-103">Konfigurace aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f883e-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="f883e-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [označit Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [ADAM Roth](https://github.com/danroth27), a [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f883e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f883e-105">Rozhraní API konfigurace poskytuje způsob, jak nakonfigurovat ASP.NET Core webové aplikace založené na seznam dvojic název hodnota.</span><span class="sxs-lookup"><span data-stu-id="f883e-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="f883e-106">Konfigurace je pro čtení, v době běhu z více zdrojů.</span><span class="sxs-lookup"><span data-stu-id="f883e-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="f883e-107">Dvojice název hodnota, je možné seskupit do víceúrovňovou hierarchii.</span><span class="sxs-lookup"><span data-stu-id="f883e-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="f883e-108">Existují zprostředkovatelé konfigurace pro:</span><span class="sxs-lookup"><span data-stu-id="f883e-108">There are configuration providers for:</span></span>

* <span data-ttu-id="f883e-109">Formáty souborů (INI, JSON a XML).</span><span class="sxs-lookup"><span data-stu-id="f883e-109">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="f883e-110">Argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f883e-110">Command-line arguments.</span></span>
* <span data-ttu-id="f883e-111">Proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="f883e-111">Environment variables.</span></span>
* <span data-ttu-id="f883e-112">Objekty .NET v paměti.</span><span class="sxs-lookup"><span data-stu-id="f883e-112">In-memory .NET objects.</span></span>
* <span data-ttu-id="f883e-113">Nešifrované [tajný klíč správce](xref:security/app-secrets) úložiště.</span><span class="sxs-lookup"><span data-stu-id="f883e-113">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="f883e-114">Šifrované uživatel uložit, například [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="f883e-114">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="f883e-115">Vlastní zprostředkovatelé (nainstalována nebo vytvořili).</span><span class="sxs-lookup"><span data-stu-id="f883e-115">Custom providers (installed or created).</span></span>

<span data-ttu-id="f883e-116">Každá hodnota konfigurace se mapuje na řetězec klíč.</span><span class="sxs-lookup"><span data-stu-id="f883e-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="f883e-117">Je dostupná podpora předdefinované vazby k deserializaci nastavení do vlastní [objektů POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objektu (jednoduché rozhraní .NET třídu s vlastnostmi).</span><span class="sxs-lookup"><span data-stu-id="f883e-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="f883e-118">Vzor možnosti používá k reprezentování skupiny související nastavení možnosti třídy.</span><span class="sxs-lookup"><span data-stu-id="f883e-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="f883e-119">Další informace o použití vzoru možnosti najdete v tématu [možnosti](xref:fundamentals/configuration/options) tématu.</span><span class="sxs-lookup"><span data-stu-id="f883e-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="f883e-120">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f883e-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="f883e-121">Konfigurace JSON</span><span class="sxs-lookup"><span data-stu-id="f883e-121">JSON configuration</span></span>

<span data-ttu-id="f883e-122">Následující konzolové aplikace používá zprostředkovatele konfigurace JSON:</span><span class="sxs-lookup"><span data-stu-id="f883e-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="f883e-123">Aplikace načte a zobrazí následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="f883e-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="f883e-124">Konfigurace se skládá z hierarchický seznam dvojic název hodnota, ve kterých jsou uzly oddělené dvojtečkou.</span><span class="sxs-lookup"><span data-stu-id="f883e-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="f883e-125">Pokud chcete načíst hodnotu, přístup k `Configuration` indexer klíčem odpovídající položky:</span><span class="sxs-lookup"><span data-stu-id="f883e-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="f883e-126">Pro práci s pole ve formátu JSON konfigurace zdrojů, použijte pole indexu jako součást řetězce oddělené dvojtečkou.</span><span class="sxs-lookup"><span data-stu-id="f883e-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="f883e-127">Následující příklad načte název první položky v předchozím `wizards` pole:</span><span class="sxs-lookup"><span data-stu-id="f883e-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="f883e-128">Dvojice název hodnota, které jsou zapsány do vestavěné [konfigurace](/dotnet/api/microsoft.extensions.configuration) poskytovatelé jsou **není** nastavené jako trvalé.</span><span class="sxs-lookup"><span data-stu-id="f883e-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="f883e-129">Však lze vytvořit vlastní zprostředkovatele, který uloží hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f883e-129">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="f883e-130">V tématu [vlastního poskytovatele konfigurace](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="f883e-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="f883e-131">V předchozím příkladu používá konfigurace indexeru načíst hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f883e-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="f883e-132">Získat přístup ke konfiguraci mimo `Startup`, použijte *možnosti vzor*.</span><span class="sxs-lookup"><span data-stu-id="f883e-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="f883e-133">Další informace najdete v tématu [možnosti](xref:fundamentals/configuration/options) tématu.</span><span class="sxs-lookup"><span data-stu-id="f883e-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="f883e-134">Konfigurační soubor XML</span><span class="sxs-lookup"><span data-stu-id="f883e-134">XML configuration</span></span>

<span data-ttu-id="f883e-135">Chcete-li pracovat s pole ve formátu XML konfigurace zdrojů, zadat `name` index pro každý prvek.</span><span class="sxs-lookup"><span data-stu-id="f883e-135">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="f883e-136">Index použijte pro přístup k hodnoty:</span><span class="sxs-lookup"><span data-stu-id="f883e-136">Use the index to access the values:</span></span>

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

## <a name="configuration-by-environment"></a><span data-ttu-id="f883e-137">Konfigurace prostředí</span><span class="sxs-lookup"><span data-stu-id="f883e-137">Configuration by environment</span></span>

<span data-ttu-id="f883e-138">Je typické jinou konfiguraci nastavení pro různá prostředí, například vývoj, testování a provozním.</span><span class="sxs-lookup"><span data-stu-id="f883e-138">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="f883e-139">`CreateDefaultBuilder` v aplikaci ASP.NET Core 2.x – metoda rozšíření (nebo pomocí `AddJsonFile` a `AddEnvironmentVariables` přímo v aplikaci ASP.NET Core 1.x) přidá zprostředkovatele konfigurace pro čtení soubory JSON a systému konfigurace zdrojů:</span><span class="sxs-lookup"><span data-stu-id="f883e-139">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="f883e-140">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="f883e-140">*appsettings.json*</span></span>
* <span data-ttu-id="f883e-141">*appsettings.\<EnvironmentName>.json*</span><span class="sxs-lookup"><span data-stu-id="f883e-141">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="f883e-142">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="f883e-142">Environment variables</span></span>

<span data-ttu-id="f883e-143">Aplikace ASP.NET Core 1.x muset volat `AddJsonFile` a [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="f883e-143">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="f883e-144">V tématu [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) vysvětlení parametrů.</span><span class="sxs-lookup"><span data-stu-id="f883e-144">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="f883e-145">`reloadOnChange` je podporován pouze v ASP.NET Core 1.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f883e-145">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="f883e-146">Konfigurace zdroje se čtou v pořadí, zda jste zadali.</span><span class="sxs-lookup"><span data-stu-id="f883e-146">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="f883e-147">V předchozím kódu se čtou poslední proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="f883e-147">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="f883e-148">Všechny hodnoty konfigurace nastavit prostřednictvím prostředí nahradit nastavené v dva poskytovatelé předchozí.</span><span class="sxs-lookup"><span data-stu-id="f883e-148">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="f883e-149">Vezměte v úvahu následující *appsettings. Staging.JSON* souboru:</span><span class="sxs-lookup"><span data-stu-id="f883e-149">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[](index/sample/appsettings.Staging.json)]

<span data-ttu-id="f883e-150">Pokud se nastaví prostředí `Staging`, následující `Configure` metoda přečte hodnotu `MyConfig`:</span><span class="sxs-lookup"><span data-stu-id="f883e-150">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


<span data-ttu-id="f883e-151">V prostředí se obvykle nastavuje na `Development`, `Staging`, nebo `Production`.</span><span class="sxs-lookup"><span data-stu-id="f883e-151">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="f883e-152">Další informace najdete v tématu [práce s několika prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="f883e-152">For more information, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="f883e-153">Požadavky na konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="f883e-153">Configuration considerations:</span></span>

* <span data-ttu-id="f883e-154">`IOptionsSnapshot` můžete znovu načíst konfigurační data, kdy se změní.</span><span class="sxs-lookup"><span data-stu-id="f883e-154">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="f883e-155">Další informace najdete v tématu [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span><span class="sxs-lookup"><span data-stu-id="f883e-155">For more information, see [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span></span>
* <span data-ttu-id="f883e-156">Konfigurace klíče jsou **není** malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="f883e-156">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="f883e-157">**Nikdy** ukládání hesel nebo jiných citlivých dat. kód zprostředkovatele konfigurace nebo v konfiguračních souborech na prostý text.</span><span class="sxs-lookup"><span data-stu-id="f883e-157">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="f883e-158">Nechcete používat produkční tajných klíčů v vývoj nebo testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="f883e-158">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="f883e-159">Zadejte tajné klíče mimo projekt tak, že nemohou být omylem zavazuje úložiště zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="f883e-159">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="f883e-160">Další informace o [práce s několika prostředí](xref:fundamentals/environments) a správu [bezpečného úložiště tajné klíče aplikace během vývoje](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="f883e-160">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="f883e-161">Pokud dvojtečkou (`:`) nelze použít v seznamu proměnných prostředí v systému, nahraďte dvojtečkou (`:`) s dvojité podtržítko (`__`).</span><span class="sxs-lookup"><span data-stu-id="f883e-161">If a colon (`:`) can't be used in environment variables on a system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="f883e-162">Zprostředkovatel v paměti a vazbu na třídu objektů POCO</span><span class="sxs-lookup"><span data-stu-id="f883e-162">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="f883e-163">Následující příklad ukazuje, jak použít poskytovatele v paměti a vytvořte vazbu na třídu:</span><span class="sxs-lookup"><span data-stu-id="f883e-163">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[](index/sample/InMemory/Program.cs)]

<span data-ttu-id="f883e-164">Hodnoty konfigurace se vrátí jako řetězce, ale vazba umožňuje konstrukce objektů.</span><span class="sxs-lookup"><span data-stu-id="f883e-164">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="f883e-165">Vazba umožňuje načtení objektů POCO nebo grafy i celý objekt.</span><span class="sxs-lookup"><span data-stu-id="f883e-165">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="f883e-166">GetValue</span><span class="sxs-lookup"><span data-stu-id="f883e-166">GetValue</span></span>

<span data-ttu-id="f883e-167">Následující příklad ukazuje [GetValue&lt;T&gt; ](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="f883e-167">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="f883e-168">ConfigurationBinder `GetValue<T>` metoda umožňuje specifikaci výchozí hodnotu (80 v ukázce).</span><span class="sxs-lookup"><span data-stu-id="f883e-168">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="f883e-169">`GetValue<T>` je pro jednoduché scénáře a bez vazby na celý části.</span><span class="sxs-lookup"><span data-stu-id="f883e-169">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="f883e-170">`GetValue<T>` Získá skalárních hodnot z `GetSection(key).Value` převést na konkrétního typu.</span><span class="sxs-lookup"><span data-stu-id="f883e-170">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="f883e-171">Vytvořit vazbu na grafu objektu</span><span class="sxs-lookup"><span data-stu-id="f883e-171">Bind to an object graph</span></span>

<span data-ttu-id="f883e-172">Každý objekt v třídě může být rekurzivně vázána.</span><span class="sxs-lookup"><span data-stu-id="f883e-172">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="f883e-173">Vezměte v úvahu následující `AppSettings` třídy:</span><span class="sxs-lookup"><span data-stu-id="f883e-173">Consider the following `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="f883e-174">Následující příklad vytvoří vazbu `AppSettings` třídy:</span><span class="sxs-lookup"><span data-stu-id="f883e-174">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="f883e-175">**ASP.NET Core 1.1** a vyšší můžete použít `Get<T>`, který pracuje s celé oddíly.</span><span class="sxs-lookup"><span data-stu-id="f883e-175">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="f883e-176">`Get<T>` může být vhodnější než použití `Bind`.</span><span class="sxs-lookup"><span data-stu-id="f883e-176">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="f883e-177">Následující kód ukazuje, jak používat `Get<T>` s v předchozím příkladu:</span><span class="sxs-lookup"><span data-stu-id="f883e-177">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="f883e-178">Pomocí následujících *appSettings.JSON určený* souboru:</span><span class="sxs-lookup"><span data-stu-id="f883e-178">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="f883e-179">Program zobrazí `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="f883e-179">The program displays `Height 11`.</span></span>

<span data-ttu-id="f883e-180">Následující kód slouží k jednotce otestovat konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="f883e-180">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="f883e-181">Vytvoření vlastního zprostředkovatele Entity Framework</span><span class="sxs-lookup"><span data-stu-id="f883e-181">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="f883e-182">V této části se vytvoří základní konfiguraci poskytovatele, který čte dvojice název hodnota v databázi pomocí EF.</span><span class="sxs-lookup"><span data-stu-id="f883e-182">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="f883e-183">Definování `ConfigurationValue` entity pro ukládání hodnoty konfigurace v databázi:</span><span class="sxs-lookup"><span data-stu-id="f883e-183">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="f883e-184">Přidat `ConfigurationContext` ukládání a přístup k nakonfigurované hodnoty:</span><span class="sxs-lookup"><span data-stu-id="f883e-184">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="f883e-185">Vytvořte třídu, která implementuje [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span><span class="sxs-lookup"><span data-stu-id="f883e-185">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="f883e-186">Vytvoření vlastního poskytovatele konfigurace dědění ze [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="f883e-186">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="f883e-187">Poskytovatel konfigurace inicializuje databázi, pokud je prázdné:</span><span class="sxs-lookup"><span data-stu-id="f883e-187">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="f883e-188">Při spuštění ukázky, zobrazí se zvýrazněné hodnoty z databáze ("value_from_ef_1" a "value_from_ef_2").</span><span class="sxs-lookup"><span data-stu-id="f883e-188">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="f883e-189">`EFConfigSource` Rozšíření metodu pro přidání zdroj konfigurace je možné použít:</span><span class="sxs-lookup"><span data-stu-id="f883e-189">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="f883e-190">Následující kód ukazuje, jak používat vlastní `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="f883e-190">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="f883e-191">Poznámka: Ukázka přidá vlastní `EFConfigProvider` po poskytovatele JSON, takže všechna nastavení z databáze přepíší nastavení z *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="f883e-191">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="f883e-192">Pomocí následujících *appSettings.JSON určený* souboru:</span><span class="sxs-lookup"><span data-stu-id="f883e-192">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="f883e-193">Zobrazí se následující výstup:</span><span class="sxs-lookup"><span data-stu-id="f883e-193">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="f883e-194">Poskytovatel konfigurace příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="f883e-194">CommandLine configuration provider</span></span>

<span data-ttu-id="f883e-195">[Poskytovatele konfigurace CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) obdrží páry klíč hodnota argument příkazového řádku pro konfiguraci za běhu.</span><span class="sxs-lookup"><span data-stu-id="f883e-195">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="f883e-196">Zobrazení nebo stažení ukázky konfigurace příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="f883e-196">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="f883e-197">Instalační program a používají zprostředkovatele konfigurace příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="f883e-197">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="f883e-198">Základní konfigurace</span><span class="sxs-lookup"><span data-stu-id="f883e-198">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="f883e-199">Chcete-li aktivovat konfigurace příkazového řádku, zavolejte `AddCommandLine` rozšiřující metody na instanci [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="f883e-199">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="f883e-200">Spuštění kódu, zobrazí se následující výstup:</span><span class="sxs-lookup"><span data-stu-id="f883e-200">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="f883e-201">Předání argumentů páry klíč hodnota na příkazovém řádku změní hodnoty `Profile:MachineName` a `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="f883e-201">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="f883e-202">V okně konzoly zobrazí:</span><span class="sxs-lookup"><span data-stu-id="f883e-202">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="f883e-203">Chcete-li přepsat konfiguraci poskytované jiných poskytovatelů konfigurace s konfigurací příkazového řádku, volejte `AddCommandLine` poslední na `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f883e-203">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f883e-204">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="f883e-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f883e-205">Typická aplikace ASP.NET Core 2.x použít metodu statické pohodlí `CreateDefaultBuilder` k sestavení hostitele:</span><span class="sxs-lookup"><span data-stu-id="f883e-205">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="f883e-206">`CreateDefaultBuilder` načte volitelné konfiguraci z *appSettings.JSON určený*, *appsettings. { Prostředí} .json*, [tajné klíče uživatele](xref:security/app-secrets) (v `Development` prostředí), proměnné prostředí a argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f883e-206">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="f883e-207">Poskytovatel konfigurace příkazového řádku se označuje jako poslední.</span><span class="sxs-lookup"><span data-stu-id="f883e-207">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="f883e-208">Poslední volání zprostředkovatele umožňuje dříve názvem argumenty příkazového řádku předaný běhu přepsat konfiguraci nastavit pomocí jiných poskytovatelů konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f883e-208">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="f883e-209">Pro *appsettings* soubory kde:</span><span class="sxs-lookup"><span data-stu-id="f883e-209">For *appsettings* files where:</span></span>

* <span data-ttu-id="f883e-210">`reloadOnChange` je povolené.</span><span class="sxs-lookup"><span data-stu-id="f883e-210">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="f883e-211">Obsahovat stejnému nastavení v argumenty příkazového řádku a *appsettings* souboru.</span><span class="sxs-lookup"><span data-stu-id="f883e-211">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="f883e-212">*Appsettings* souboru, který obsahuje odpovídající argument příkazového řádku je změnit po spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="f883e-212">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="f883e-213">Pokud jsou splněny všechny předchozí podmínky, se přepíšou argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f883e-213">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="f883e-214">Můžete použít aplikaci ASP.NET Core 2.x [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) místo `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f883e-214">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f883e-215">Při použití `WebHostBuilder`, je nutné ručně nastavit konfiguraci s [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="f883e-215">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="f883e-216">V tématu kartě ASP.NET Core 1.x pro další informace.</span><span class="sxs-lookup"><span data-stu-id="f883e-216">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f883e-217">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="f883e-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f883e-218">Vytvoření [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) a volání `AddCommandLine` metodu použít poskytovatele konfigurace příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f883e-218">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="f883e-219">Poslední volání zprostředkovatele umožňuje dříve názvem argumenty příkazového řádku předaný běhu přepsat konfiguraci nastavit pomocí jiných poskytovatelů konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f883e-219">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="f883e-220">Použít konfiguraci [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) s `UseConfiguration` metoda:</span><span class="sxs-lookup"><span data-stu-id="f883e-220">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="f883e-221">Arguments</span><span class="sxs-lookup"><span data-stu-id="f883e-221">Arguments</span></span>

<span data-ttu-id="f883e-222">Argumenty předávané na příkazovém řádku musí odpovídat jednomu ze dvou formátů uvedené v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="f883e-222">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="f883e-223">Argument formátu</span><span class="sxs-lookup"><span data-stu-id="f883e-223">Argument format</span></span>                                                     | <span data-ttu-id="f883e-224">Příklad</span><span class="sxs-lookup"><span data-stu-id="f883e-224">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="f883e-225">Jeden argument: pár klíč hodnota oddělená symbolem rovná se (`=`)</span><span class="sxs-lookup"><span data-stu-id="f883e-225">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="f883e-226">Pořadí dva argumenty: pár klíč hodnota oddělené mezerou</span><span class="sxs-lookup"><span data-stu-id="f883e-226">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="f883e-227">**Jeden argument**</span><span class="sxs-lookup"><span data-stu-id="f883e-227">**Single argument**</span></span>

<span data-ttu-id="f883e-228">Hodnota musí následovat znak rovná se (`=`).</span><span class="sxs-lookup"><span data-stu-id="f883e-228">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="f883e-229">Hodnota může mít hodnotu null (například `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="f883e-229">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="f883e-230">Klíč může mít předponu.</span><span class="sxs-lookup"><span data-stu-id="f883e-230">The key may have a prefix.</span></span>

| <span data-ttu-id="f883e-231">Předpona klíče</span><span class="sxs-lookup"><span data-stu-id="f883e-231">Key prefix</span></span>               | <span data-ttu-id="f883e-232">Příklad</span><span class="sxs-lookup"><span data-stu-id="f883e-232">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="f883e-233">Žádná předpona.</span><span class="sxs-lookup"><span data-stu-id="f883e-233">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="f883e-234">Jednotné dash (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="f883e-234">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="f883e-235">Dvě pomlčky (`--`)</span><span class="sxs-lookup"><span data-stu-id="f883e-235">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="f883e-236">Lomítko (`/`)</span><span class="sxs-lookup"><span data-stu-id="f883e-236">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="f883e-237">&#8224;Klíč s předponou jediného (`-`) musí být zadáno v [přepínač mapování](#switch-mappings), které jsou popsány níže.</span><span class="sxs-lookup"><span data-stu-id="f883e-237">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="f883e-238">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f883e-238">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="f883e-239">Poznámka: Pokud `-key1` není součástí [přepínač mapování](#switch-mappings) předaná zprostředkovateli konfigurace `FormatException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="f883e-239">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="f883e-240">**Pořadí dva argumenty**</span><span class="sxs-lookup"><span data-stu-id="f883e-240">**Sequence of two arguments**</span></span>

<span data-ttu-id="f883e-241">Hodnota nesmí být null a musí následovat klíč oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="f883e-241">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="f883e-242">Klíč musí mít předponu.</span><span class="sxs-lookup"><span data-stu-id="f883e-242">The key must have a prefix.</span></span>

| <span data-ttu-id="f883e-243">Předpona klíče</span><span class="sxs-lookup"><span data-stu-id="f883e-243">Key prefix</span></span>               | <span data-ttu-id="f883e-244">Příklad</span><span class="sxs-lookup"><span data-stu-id="f883e-244">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="f883e-245">Jednotné dash (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="f883e-245">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="f883e-246">Dvě pomlčky (`--`)</span><span class="sxs-lookup"><span data-stu-id="f883e-246">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="f883e-247">Lomítko (`/`)</span><span class="sxs-lookup"><span data-stu-id="f883e-247">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="f883e-248">&#8224;Klíč s předponou jediného (`-`) musí být zadáno v [přepínač mapování](#switch-mappings), které jsou popsány níže.</span><span class="sxs-lookup"><span data-stu-id="f883e-248">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="f883e-249">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f883e-249">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="f883e-250">Poznámka: Pokud `-key1` není součástí [přepínač mapování](#switch-mappings) předaná zprostředkovateli konfigurace `FormatException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="f883e-250">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="f883e-251">Duplicitní klíče</span><span class="sxs-lookup"><span data-stu-id="f883e-251">Duplicate keys</span></span>

<span data-ttu-id="f883e-252">Pokud duplicitní klíče jsou k dispozici, použije se poslední dvojice klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="f883e-252">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="f883e-253">Mapování přepínače</span><span class="sxs-lookup"><span data-stu-id="f883e-253">Switch mappings</span></span>

<span data-ttu-id="f883e-254">Při ruční vytváření konfigurací s `ConfigurationBuilder`, slovník mapování přepínače lze přidat do `AddCommandLine` metoda.</span><span class="sxs-lookup"><span data-stu-id="f883e-254">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="f883e-255">Mapování přepínač Povolit logiku nahrazení název klíče.</span><span class="sxs-lookup"><span data-stu-id="f883e-255">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="f883e-256">Když se používá slovník mapování přepínače, se kontroluje slovníku pro klíč, který se shoduje s klíčem poskytované argument příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f883e-256">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="f883e-257">Pokud je nalezen příkazového řádku klíč ve slovníku, hodnota slovníku (klíče nahrazení) je předán zpět k nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f883e-257">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="f883e-258">Mapování přepínač je vyžadována pro všechny klíč příkazového řádku s předponou jeden pomlčkou (`-`).</span><span class="sxs-lookup"><span data-stu-id="f883e-258">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="f883e-259">Přepínač pravidla klíče slovníku mapování:</span><span class="sxs-lookup"><span data-stu-id="f883e-259">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="f883e-260">Přepínače musí začínat pomlčkou (`-`) nebo dvojitou pomlčkou (`--`).</span><span class="sxs-lookup"><span data-stu-id="f883e-260">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="f883e-261">Slovník mapování přepínač nesmí obsahovat duplicitní klíče.</span><span class="sxs-lookup"><span data-stu-id="f883e-261">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="f883e-262">V následujícím příkladu `GetSwitchMappings` metoda umožňuje argumentů příkazového řádku pro použití jedné pomlčkou (`-`) klíče předponu a vyhnout se počáteční podklíčů předpony.</span><span class="sxs-lookup"><span data-stu-id="f883e-262">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="f883e-263">Bez zadání argumentů příkazového řádku, slovníku poskytované `AddInMemoryCollection` nastaví hodnoty konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f883e-263">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="f883e-264">Spusťte aplikaci pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="f883e-264">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="f883e-265">V okně konzoly zobrazí:</span><span class="sxs-lookup"><span data-stu-id="f883e-265">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="f883e-266">Použijte následující postupy k předávání v nastavení konfigurace:</span><span class="sxs-lookup"><span data-stu-id="f883e-266">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="f883e-267">V okně konzoly zobrazí:</span><span class="sxs-lookup"><span data-stu-id="f883e-267">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="f883e-268">Po vytvoření slovníku mapování přepínač obsahuje data zobrazená v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="f883e-268">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="f883e-269">Key</span><span class="sxs-lookup"><span data-stu-id="f883e-269">Key</span></span>            | <span data-ttu-id="f883e-270">Hodnota</span><span class="sxs-lookup"><span data-stu-id="f883e-270">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="f883e-271">K předvedení klíče přepínání pomocí slovníku, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f883e-271">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="f883e-272">Příkazového řádku klíče jsou vzájemně zaměněny.</span><span class="sxs-lookup"><span data-stu-id="f883e-272">The command-line keys are swapped.</span></span> <span data-ttu-id="f883e-273">V okně konzoly zobrazí hodnoty konfigurace pro `Profile:MachineName` a `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="f883e-273">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="f883e-274">web.config file</span><span class="sxs-lookup"><span data-stu-id="f883e-274">web.config file</span></span>

<span data-ttu-id="f883e-275">A *web.config* soubor je požadován při hostování aplikace v IIS nebo IIS Express.</span><span class="sxs-lookup"><span data-stu-id="f883e-275">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="f883e-276">Nastavení v *web.config* povolit [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) spusťte aplikaci a nakonfigurovat další nastavení služby IIS a modulů.</span><span class="sxs-lookup"><span data-stu-id="f883e-276">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="f883e-277">Pokud *web.config* soubor není přítomen a zahrnuje soubor projektu `<Project Sdk="Microsoft.NET.Sdk.Web">`, publikování projektu vytvoří *web.config* souboru v publikované výstup ( *publikování* složku).</span><span class="sxs-lookup"><span data-stu-id="f883e-277">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="f883e-278">Další informace najdete v tématu [hostitele ASP.NET Core v systému Windows pomocí služby IIS](xref:host-and-deploy/iis/index#webconfig-file).</span><span class="sxs-lookup"><span data-stu-id="f883e-278">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="accessing-configuration-during-startup"></a><span data-ttu-id="f883e-279">Přístup k konfigurace při spuštění</span><span class="sxs-lookup"><span data-stu-id="f883e-279">Accessing configuration during startup</span></span>

<span data-ttu-id="f883e-280">Získat přístup ke konfiguraci v rámci `ConfigureServices` nebo `Configure` během spouštění, podívejte se na příklady v [spuštění aplikace](xref:fundamentals/startup) tématu.</span><span class="sxs-lookup"><span data-stu-id="f883e-280">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="f883e-281">Další poznámky</span><span class="sxs-lookup"><span data-stu-id="f883e-281">Additional notes</span></span>

* <span data-ttu-id="f883e-282">Vkládání závislostí (DI) není nastavená až do doby, po `ConfigureServices` je volána.</span><span class="sxs-lookup"><span data-stu-id="f883e-282">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="f883e-283">Konfigurace systému není DI vědět.</span><span class="sxs-lookup"><span data-stu-id="f883e-283">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="f883e-284">`IConfiguration` má dva specializací:</span><span class="sxs-lookup"><span data-stu-id="f883e-284">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="f883e-285">`IConfigurationRoot` Používá se pro kořenový uzel.</span><span class="sxs-lookup"><span data-stu-id="f883e-285">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="f883e-286">Můžete aktivovat znovu načíst.</span><span class="sxs-lookup"><span data-stu-id="f883e-286">Can trigger a reload.</span></span>
  * <span data-ttu-id="f883e-287">`IConfigurationSection` Reprezentuje oddíl hodnoty konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f883e-287">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="f883e-288">`GetSection` a `GetChildren` metody vrací `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="f883e-288">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="f883e-289">Použití [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) při opětovném načtení konfigurace nebo pro přístup do každého poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="f883e-289">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="f883e-290">Ani jeden z těchto situacích jsou běžné.</span><span class="sxs-lookup"><span data-stu-id="f883e-290">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f883e-291">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f883e-291">Additional resources</span></span>

* [<span data-ttu-id="f883e-292">Možnosti</span><span class="sxs-lookup"><span data-stu-id="f883e-292">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="f883e-293">Práce s několika prostředí</span><span class="sxs-lookup"><span data-stu-id="f883e-293">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="f883e-294">Bezpečné úložiště tajných částí aplikace při vývoji</span><span class="sxs-lookup"><span data-stu-id="f883e-294">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="f883e-295">Hostování v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f883e-295">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="f883e-296">Injektáž závislostí</span><span class="sxs-lookup"><span data-stu-id="f883e-296">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="f883e-297">Zprostředkovatel konfigurace služby Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f883e-297">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
