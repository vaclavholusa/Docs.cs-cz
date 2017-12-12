---
title: "Vzor možnosti v ASP.NET Core"
author: guardrex
description: "Zjistit, jak používat možnosti vzor k reprezentování skupiny související nastavení v aplikacích ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 11/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/options
ms.openlocfilehash: 7d89416626433bf737b63eda4b17e65b089ae142
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="5aae4-103">Vzor možnosti v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5aae4-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="5aae4-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5aae4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5aae4-105">Vzor možnosti používá k reprezentování skupiny související nastavení možnosti třídy.</span><span class="sxs-lookup"><span data-stu-id="5aae4-105">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="5aae4-106">Při nastavení konfigurace jsou izolované funkce do třídy samostatné možnosti, aplikace dodržuje zásady dvě důležité inženýrství softwaru:</span><span class="sxs-lookup"><span data-stu-id="5aae4-106">When configuration settings are isolated by feature into separate options classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="5aae4-107">[Rozhraní oddělení princip (ISP)](http://deviq.com/interface-segregation-principle/): funkce (třídy), které závisí na nastavení konfigurace závisí na nastavení konfigurace, která používají jenom.</span><span class="sxs-lookup"><span data-stu-id="5aae4-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="5aae4-108">[Oddělené oblasti zájmu](http://deviq.com/separation-of-concerns/): nastavení pro různé části aplikace nejsou závislé nebo párované jednu na druhou.</span><span class="sxs-lookup"><span data-stu-id="5aae4-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="5aae4-109">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)) Tento článek je snazší postupujte podle ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5aae4-109">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="5aae4-110">Základní možnosti konfigurace</span><span class="sxs-lookup"><span data-stu-id="5aae4-110">Basic options configuration</span></span>

<span data-ttu-id="5aae4-111">Základní možnosti konfigurace je znázorněn příklad &num;1 v [ukázkovou aplikaci](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5aae4-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5aae4-112">Třída možností musí být neabstraktní s konstruktor public bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="5aae4-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="5aae4-113">Následující třídy, `MyOptions`, má dvě vlastnosti `Option1` a `Option2`.</span><span class="sxs-lookup"><span data-stu-id="5aae4-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="5aae4-114">Nastavení výchozích hodnot je nepovinný, ale konstruktoru třídy v následujícím příkladu se nastaví na výchozí hodnotu `Option1`.</span><span class="sxs-lookup"><span data-stu-id="5aae4-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="5aae4-115">`Option2`Výchozí hodnota je nastavena pomocí vlastnosti inicializace přímo (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="5aae4-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="5aae4-116">`MyOptions` Třída je přidat do kontejneru služby s [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) a vázaný k konfigurace:</span><span class="sxs-lookup"><span data-stu-id="5aae4-116">The `MyOptions` class is added to the service container with [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) and bound to configuration:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="5aae4-117">Následující stránka používá model [vkládání závislostí konstruktor](xref:fundamentals/dependency-injection#what-is-dependency-injection) s [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) pro přístup k nastavení (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5aae4-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="5aae4-118">Ukázkových *appSettings.JSON určený* soubor Určuje hodnoty pro `option1` a `option2`:</span><span class="sxs-lookup"><span data-stu-id="5aae4-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[Main](options/sample/appsettings.json)]

<span data-ttu-id="5aae4-119">Pokud aplikace běží a model stránky `OnGet` metoda vrátí řetězec zobrazující třída hodnoty možnosti:</span><span class="sxs-lookup"><span data-stu-id="5aae4-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="5aae4-120">Jednoduché možnosti nakonfigurovat delegáta</span><span class="sxs-lookup"><span data-stu-id="5aae4-120">Configure simple options with a delegate</span></span>

<span data-ttu-id="5aae4-121">Jednoduché možnosti konfigurace s delegáta je znázorněn příklad &num;2 v [ukázkovou aplikaci](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5aae4-121">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5aae4-122">Použití delegáta pro nastavení možností hodnot.</span><span class="sxs-lookup"><span data-stu-id="5aae4-122">Use a delegate to set options values.</span></span> <span data-ttu-id="5aae4-123">Použití ukázkové aplikace `MyOptionsWithDelegateConfig` – třída (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="5aae4-123">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="5aae4-124">V následujícím kódu druhý `IConfigureOptions<TOptions>` služby se přidá do kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="5aae4-124">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="5aae4-125">Ke konfiguraci vazby se používá delegáta `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="5aae4-125">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="5aae4-126">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="5aae4-126">*Index.cshtml.cs*:</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="5aae4-127">Můžete přidat několik poskytovatelů konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5aae4-127">You can add multiple configuration providers.</span></span> <span data-ttu-id="5aae4-128">Poskytovatelé konfigurace jsou k dispozici v balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="5aae4-128">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="5aae4-129">Uplatňují se, aby bylo úspěšné registrace.</span><span class="sxs-lookup"><span data-stu-id="5aae4-129">They're applied in order that they're registered.</span></span>

<span data-ttu-id="5aae4-130">Každé volání [konfigurace&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) přidá `IConfigureOptions<TOptions>` službu kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="5aae4-130">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="5aae4-131">V předchozím příkladu, hodnoty `Option1` a `Option2` jsou určené v *appSettings.JSON určený*, ale hodnoty `Option1` a `Option2` jsou přepsány nakonfigurované delegáta.</span><span class="sxs-lookup"><span data-stu-id="5aae4-131">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="5aae4-132">Pokud je povoleno více než jedna služba konfigurace, poslední zdroj konfigurace specifikovat *wins* a nastaví hodnotu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5aae4-132">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="5aae4-133">Pokud aplikace běží a model stránky `OnGet` metoda vrátí řetězec zobrazující třída hodnoty možnosti:</span><span class="sxs-lookup"><span data-stu-id="5aae4-133">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="5aae4-134">Konfigurace suboptions</span><span class="sxs-lookup"><span data-stu-id="5aae4-134">Suboptions configuration</span></span>

<span data-ttu-id="5aae4-135">Konfigurace suboptions je znázorněn příklad &num;3 v [ukázkovou aplikaci](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5aae4-135">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5aae4-136">Aplikace by měl vytvořit možnosti třídy, které se vztahují k příslušné funkce skupiny (třídy) v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5aae4-136">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="5aae4-137">Části aplikace, které vyžadují hodnoty konfigurace pouze měli přístup k hodnotám konfigurace, které používají.</span><span class="sxs-lookup"><span data-stu-id="5aae4-137">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="5aae4-138">Při vytváření vazby možnosti konfigurace, každou vlastnost v možnosti typ je vázán na konfigurační klíč ve formátu `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="5aae4-138">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="5aae4-139">Například `MyOptions.Option1` vlastnost je vázána na klíč `Option1`, který je pro čtení z `option1` vlastnost *appSettings.JSON určený*.</span><span class="sxs-lookup"><span data-stu-id="5aae4-139">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="5aae4-140">V následujícím kódu, třetí `IConfigureOptions<TOptions>` služby se přidá do kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="5aae4-140">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="5aae4-141">Vytvoří vazbu mezi `MySubOptions` do části `subsection` z *appSettings.JSON určený* souboru:</span><span class="sxs-lookup"><span data-stu-id="5aae4-141">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="5aae4-142">`GetSection` Rozšíření metoda vyžaduje, [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="5aae4-142">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="5aae4-143">Pokud aplikace používá [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, tento balíček je automaticky zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="5aae4-143">If the app uses the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, the package is automatically included.</span></span>

<span data-ttu-id="5aae4-144">Ukázkových *appSettings.JSON určený* soubor definuje `subsection` člena s klíči pro `suboption1` a `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="5aae4-144">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="5aae4-145">`MySubOptions` Třída definuje vlastnosti, `SubOption1` a `SubOption2`, aby udržení hodnoty dílčí možnosti (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="5aae4-145">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="5aae4-146">Model stránky `OnGet` metoda vrátí řetězec s hodnotami dílčí možnost (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5aae4-146">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="5aae4-147">Při spuštění aplikace `OnGet` metoda vrátí řetězec zobrazující dílčí možnost hodnoty třídy:</span><span class="sxs-lookup"><span data-stu-id="5aae4-147">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="5aae4-148">Možnosti poskytnuté modelem zobrazení nebo pomocí vkládání přímé zobrazení</span><span class="sxs-lookup"><span data-stu-id="5aae4-148">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="5aae4-149">Možnosti poskytnuté modelem zobrazení nebo pomocí vkládání přímé zobrazení je znázorněn příklad &num;4 v [ukázkovou aplikaci](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5aae4-149">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5aae4-150">Možnosti můžete zadat v zobrazení modelu nebo vložením `IOptions<TOptions>` přímo do zobrazení (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5aae4-150">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="5aae4-151">Přímé vkládání vložit `IOptions<MyOptions>` s `@inject` – direktiva:</span><span class="sxs-lookup"><span data-stu-id="5aae4-151">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="5aae4-152">Při spuštění aplikace na vykreslené stránce se zobrazuje hodnoty možnosti:</span><span class="sxs-lookup"><span data-stu-id="5aae4-152">When the app is run, the option values are shown in the rendered page:</span></span>

![Možnosti hodnoty možnost 1: value1_from_json a Option2: -1 se načtou z modelu a vkládání do zobrazení.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="5aae4-154">Znovu načíst data konfigurace s IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="5aae4-154">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="5aae4-155">Opětovné načtení konfiguračních dat pomocí `IOptionsSnapshot` je znázorněn v příkladu &num;5 v [ukázkovou aplikaci](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5aae4-155">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5aae4-156">*Vyžaduje ASP.NET Core 1.1 nebo novější.*</span><span class="sxs-lookup"><span data-stu-id="5aae4-156">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="5aae4-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) podporuje opětovném načtení možnosti s minimální režie zpracování.</span><span class="sxs-lookup"><span data-stu-id="5aae4-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="5aae4-158">V technologii ASP.NET Core 1.1 `IOptionsSnapshot` je snímek [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) a aktualizace automaticky vždy, když monitorování aktivuje na základě změny zdroj dat změny.</span><span class="sxs-lookup"><span data-stu-id="5aae4-158">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="5aae4-159">V technologii ASP.NET Core 2.0 nebo novější možnosti se vypočítávají jednou za žádosti při přístup a uložené v mezipaměti po dobu jeho existence požadavku.</span><span class="sxs-lookup"><span data-stu-id="5aae4-159">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="5aae4-160">Následující příklad ukazuje, jak novou `IOptionsSnapshot` je vytvořen po *appSettings.JSON určený* změny (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="5aae4-160">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="5aae4-161">Víc požadavků na server vrátit konstantní hodnoty poskytované *appSettings.JSON určený* souborů, dokud se změní soubor a konfigurace znovu načte.</span><span class="sxs-lookup"><span data-stu-id="5aae4-161">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="5aae4-162">Následující obrázek ukazuje počáteční `option1` a `option2` načíst hodnoty z *appSettings.JSON určený* souboru:</span><span class="sxs-lookup"><span data-stu-id="5aae4-162">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="5aae4-163">Změňte hodnoty *appSettings.JSON určený* do souboru `value1_from_json UPDATED` a `200`.</span><span class="sxs-lookup"><span data-stu-id="5aae4-163">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="5aae4-164">Uložit *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="5aae4-164">Save the *appsettings.json* file.</span></span> <span data-ttu-id="5aae4-165">Aktualizujte prohlížeč a že jsou aktualizovány hodnoty možnosti:</span><span class="sxs-lookup"><span data-stu-id="5aae4-165">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="5aae4-166">Možnosti podpory s IConfigureNamedOptions s názvem</span><span class="sxs-lookup"><span data-stu-id="5aae4-166">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="5aae4-167">S názvem podporu možnosti s [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) je znázorněn příklad &num;6 v [ukázkovou aplikaci](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5aae4-167">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5aae4-168">*Vyžaduje základní technologie ASP.NET 2.0 nebo novější.*</span><span class="sxs-lookup"><span data-stu-id="5aae4-168">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="5aae4-169">*S názvem možnosti* podpory umožňuje aplikaci k rozlišení mezi pojmenované možnosti konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5aae4-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="5aae4-170">V ukázkové aplikace, jsou pojmenované možnosti deklarovat s [ConfigureNamedOptions&lt;TOptions&gt;. Konfigurace](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) metoda:</span><span class="sxs-lookup"><span data-stu-id="5aae4-170">In the sample app, named options are declared with the [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="5aae4-171">Ukázková aplikace používá s názvem možnosti s [IOptionsSnapshot&lt;TOptions&gt;. Získat](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5aae4-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="5aae4-172">Spuštění ukázkové aplikace, jsou vráceny s názvem možnosti:</span><span class="sxs-lookup"><span data-stu-id="5aae4-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="5aae4-173">`named_options_1`hodnoty jsou uvedeny z konfigurace, které jsou načteny z *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="5aae4-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="5aae4-174">`named_options_2`hodnoty poskytuje:</span><span class="sxs-lookup"><span data-stu-id="5aae4-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="5aae4-175">`named_options_2` Delegovat v `ConfigureServices` pro `Option1`.</span><span class="sxs-lookup"><span data-stu-id="5aae4-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="5aae4-176">Výchozí hodnota pro `Option2` poskytované `MyOptions` třídy.</span><span class="sxs-lookup"><span data-stu-id="5aae4-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="5aae4-177">Nakonfigurujte všechny instance s názvem možnosti s [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) metoda.</span><span class="sxs-lookup"><span data-stu-id="5aae4-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="5aae4-178">Následující kód konfiguruje `Option1` pro všechny pojmenované instance konfigurace s hodnotou běžné.</span><span class="sxs-lookup"><span data-stu-id="5aae4-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="5aae4-179">Přidejte následující kód ručně na `Configure` metoda:</span><span class="sxs-lookup"><span data-stu-id="5aae4-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="5aae4-180">Spuštění ukázkové aplikace po přidání kód vytvoří následující výsledek:</span><span class="sxs-lookup"><span data-stu-id="5aae4-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="5aae4-181">V technologii ASP.NET Core 2.0 nebo novější jsou všechny možnosti s názvem instance.</span><span class="sxs-lookup"><span data-stu-id="5aae4-181">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="5aae4-182">Existující `IConfigureOption` instance jsou považovány za cílení `Options.DefaultName` instance, kterou je `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="5aae4-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="5aae4-183">`IConfigureNamedOptions`také implementuje `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="5aae4-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="5aae4-184">Výchozí implementaci [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([odkaz na zdroj](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) obsahuje logiku a odpovídajícím způsobem používat.</span><span class="sxs-lookup"><span data-stu-id="5aae4-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) has logic to use each appropriately.</span></span> <span data-ttu-id="5aae4-185">`null` Pojmenované možnost se používá pro všechny pojmenované instance, místo konkrétního pojmenovanou instanci ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) a [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) použít tento názvů).</span><span class="sxs-lookup"><span data-stu-id="5aae4-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="5aae4-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="5aae4-186">IPostConfigureOptions</span></span>

<span data-ttu-id="5aae4-187">*Vyžaduje základní technologie ASP.NET 2.0 nebo novější.*</span><span class="sxs-lookup"><span data-stu-id="5aae4-187">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="5aae4-188">Nastavit postconfiguration s [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="5aae4-188">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="5aae4-189">Postconfiguration spustí po všech [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="5aae4-189">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="5aae4-190">[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) je k dispozici po konfigurace s názvem možností:</span><span class="sxs-lookup"><span data-stu-id="5aae4-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="5aae4-191">Použití [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) po nakonfigurovat všechny pojmenované instance konfigurace:</span><span class="sxs-lookup"><span data-stu-id="5aae4-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="5aae4-192">Možnosti objektu pro vytváření, monitorování a mezipaměti</span><span class="sxs-lookup"><span data-stu-id="5aae4-192">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="5aae4-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) se používá pro oznámení při `TOptions` instance změnu.</span><span class="sxs-lookup"><span data-stu-id="5aae4-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="5aae4-194">`IOptionsMonitor`podporuje možností možnosti, oznámení, změn a `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="5aae4-194">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="5aae4-195">[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (jádro ASP.NET 2.0 nebo novější) zodpovídá za vytvoření nové možnosti instancí.</span><span class="sxs-lookup"><span data-stu-id="5aae4-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="5aae4-196">Má jeden [vytvořit](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) metoda.</span><span class="sxs-lookup"><span data-stu-id="5aae4-196">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="5aae4-197">Výchozí implementace trvá všech registrovaných `IConfigureOptions` a `IPostConfigureOptions` a spustí všechny první nakonfiguruje, za nímž následuje po konfiguruje.</span><span class="sxs-lookup"><span data-stu-id="5aae4-197">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="5aae4-198">Umožňuje rozlišovat mezi `IConfigureNamedOptions` a `IConfigureOptions` a pouze volá vhodné rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5aae4-198">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="5aae4-199">[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (jádro ASP.NET 2.0 nebo novější) používá `IOptionsMonitor` do mezipaměti `TOptions` instance.</span><span class="sxs-lookup"><span data-stu-id="5aae4-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="5aae4-200">`IOptionsMonitorCache` By způsobila neplatnost instance možnosti v monitorování tak, aby hodnota je přepočítávány ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="5aae4-200">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="5aae4-201">Hodnoty mohou být ručně zavedené i [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="5aae4-201">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="5aae4-202">[Zrušte](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) metoda se používá, když všechny pojmenované instance by měl být znovu vytvořen na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="5aae4-202">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="see-also"></a><span data-ttu-id="5aae4-203">Viz také</span><span class="sxs-lookup"><span data-stu-id="5aae4-203">See also</span></span>

* [<span data-ttu-id="5aae4-204">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="5aae4-204">Configuration</span></span>](xref:fundamentals/configuration/index)
