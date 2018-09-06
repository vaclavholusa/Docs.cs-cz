---
title: Konfigurace v ASP.NET Core
author: guardrex
description: Zjistěte, jak použít rozhraní API pro konfiguraci ke konfiguraci aplikace ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 288f8ba5b45cdecd8c9eae060fee2c2c25dec7f9
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/06/2018
ms.locfileid: "43893243"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="5b501-103">Konfigurace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b501-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="5b501-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5b501-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5b501-105">Konfigurace aplikace v ASP.NET Core je založená na páry klíč hodnota stanovené *poskytovatelé konfigurace*.</span><span class="sxs-lookup"><span data-stu-id="5b501-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="5b501-106">Poskytovatelé konfigurace čtení konfiguračních dat do párů hodnot klíčů z různých zdrojů konfigurace:</span><span class="sxs-lookup"><span data-stu-id="5b501-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="5b501-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="5b501-107">Azure Key Vault</span></span>
* <span data-ttu-id="5b501-108">Argumenty příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5b501-108">Command-line arguments</span></span>
* <span data-ttu-id="5b501-109">Vlastní zprostředkovatelé (nainstalované nebo vytváření)</span><span class="sxs-lookup"><span data-stu-id="5b501-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="5b501-110">Adresář souborů</span><span class="sxs-lookup"><span data-stu-id="5b501-110">Directory files</span></span>
* <span data-ttu-id="5b501-111">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="5b501-111">Environment variables</span></span>
* <span data-ttu-id="5b501-112">Objekty v paměti .NET</span><span class="sxs-lookup"><span data-stu-id="5b501-112">In-memory .NET objects</span></span>
* <span data-ttu-id="5b501-113">Soubory nastavení</span><span class="sxs-lookup"><span data-stu-id="5b501-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="5b501-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="5b501-114">Azure Key Vault</span></span>
* <span data-ttu-id="5b501-115">Argumenty příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5b501-115">Command-line arguments</span></span>
* <span data-ttu-id="5b501-116">Vlastní zprostředkovatelé (nainstalované nebo vytváření)</span><span class="sxs-lookup"><span data-stu-id="5b501-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="5b501-117">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="5b501-117">Environment variables</span></span>
* <span data-ttu-id="5b501-118">Objekty v paměti .NET</span><span class="sxs-lookup"><span data-stu-id="5b501-118">In-memory .NET objects</span></span>
* <span data-ttu-id="5b501-119">Soubory nastavení</span><span class="sxs-lookup"><span data-stu-id="5b501-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="5b501-120">Argumenty příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5b501-120">Command-line arguments</span></span>
* <span data-ttu-id="5b501-121">Vlastní zprostředkovatelé (nainstalované nebo vytváření)</span><span class="sxs-lookup"><span data-stu-id="5b501-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="5b501-122">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="5b501-122">Environment variables</span></span>
* <span data-ttu-id="5b501-123">Objekty v paměti .NET</span><span class="sxs-lookup"><span data-stu-id="5b501-123">In-memory .NET objects</span></span>
* <span data-ttu-id="5b501-124">Soubory nastavení</span><span class="sxs-lookup"><span data-stu-id="5b501-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="5b501-125">*Možnosti vzor* je rozšířením konfigurace koncepty popsané v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="5b501-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="5b501-126">Možnosti třídy používá k reprezentování skupiny související nastavení.</span><span class="sxs-lookup"><span data-stu-id="5b501-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="5b501-127">Další informace o použití vzoru možnosti najdete v tématu <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="5b501-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="5b501-128">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5b501-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5b501-129">Příklady uvedené v tomto tématu využívají:</span><span class="sxs-lookup"><span data-stu-id="5b501-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="5b501-130">Nastavení základní cesty aplikace s využitím <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="5b501-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="5b501-131">`SetBasePath` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="5b501-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="5b501-132">Řešení oddíly konfiguračních souborů s <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span><span class="sxs-lookup"><span data-stu-id="5b501-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="5b501-133">`GetSection` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="5b501-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="5b501-134">Konfigurace vazby na .NET třídy s <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> a [získat&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span><span class="sxs-lookup"><span data-stu-id="5b501-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="5b501-135">`Bind` a `Get<T>` jsou k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="5b501-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="5b501-136">`Get<T>` je k dispozici v ASP.NET Core 1.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5b501-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5b501-137">Tyto tři balíčky jsou součástí [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="5b501-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="5b501-138">Tyto tři balíčky jsou součástí [metabalíček Microsoft.aspnetcore.all](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="5b501-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="5b501-139">Hostování a konfigurace aplikací</span><span class="sxs-lookup"><span data-stu-id="5b501-139">Host vs. app configuration</span></span>

<span data-ttu-id="5b501-140">Předtím, než aplikace je nakonfigurovaná a spuštěna, *hostitele* nakonfigurovaný a spustit.</span><span class="sxs-lookup"><span data-stu-id="5b501-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="5b501-141">Hostitel je zodpovědný za spouštění a životního cyklu správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="5b501-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="5b501-142">Aplikace a hostitel jsou nakonfigurováni pomocí zprostředkovatele konfigurace popsané v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="5b501-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="5b501-143">Páry klíč hodnota konfigurace hostitele se stanou součástí globální konfiguraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b501-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="5b501-144">Další informace o jak konfiguraci poskytovatele se používají při vytváření hostitele a vliv zdroje konfigurace hostitele konfigurace najdete v tématu <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="5b501-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="5b501-145">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="5b501-145">Security</span></span>

<span data-ttu-id="5b501-146">Použijte následující osvědčené postupy:</span><span class="sxs-lookup"><span data-stu-id="5b501-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="5b501-147">Nikdy ukládat hesla a jiná citlivá data v konfiguraci zprostředkovatele kódu nebo v konfiguračních souborech na prostý text.</span><span class="sxs-lookup"><span data-stu-id="5b501-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="5b501-148">Nechcete používat produkční tajných kódů při vývoji nebo testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="5b501-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="5b501-149">Zadejte tajných kódů mimo projekt tak, že nemohou být omylem zaměřuje na úložiště zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="5b501-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="5b501-150">Další informace o [jak používat více prostředí](xref:fundamentals/environments) a správu [bezpečné ukládání tajných kódů aplikace při vývoji pomocí manažera tajných](xref:security/app-secrets) (včetně poradit se správným určením použití proměnných k ukládání citlivá data).</span><span class="sxs-lookup"><span data-stu-id="5b501-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="5b501-151">Správce tajný klíč používá zprostředkovatel konfigurace souboru ukládat tajné klíče uživatelů v souboru JSON v místním systému.</span><span class="sxs-lookup"><span data-stu-id="5b501-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="5b501-152">Zprostředkovatel konfigurace souboru je popsána dále v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="5b501-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="5b501-153">[Služba Azure Key Vault](https://azure.microsoft.com/services/key-vault/) je jednou z možností pro bezpečné ukládání tajných klíčů aplikací.</span><span class="sxs-lookup"><span data-stu-id="5b501-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="5b501-154">Další informace naleznete v tématu <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="5b501-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="5b501-155">Hierarchické konfigurační data</span><span class="sxs-lookup"><span data-stu-id="5b501-155">Hierarchical configuration data</span></span>

<span data-ttu-id="5b501-156">Konfigurační rozhraní API je schopni zachovat hierarchické konfigurační data linearizovat hierarchických dat s použitím oddělovače v konfigurační klíče.</span><span class="sxs-lookup"><span data-stu-id="5b501-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="5b501-157">V následujícím souboru JSON existují čtyři kódy v hierarchii strukturovaných ze dvou částí:</span><span class="sxs-lookup"><span data-stu-id="5b501-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

<span data-ttu-id="5b501-158">Pokud je soubor pro čtení do konfigurace, jedinečné klíče se vytvoří zachovat původní struktury hierarchická data zdroj konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5b501-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="5b501-159">Oddíly a klíče se sloučí s použitím dvojtečkou (`:`) Chcete-li zachovat původní struktury:</span><span class="sxs-lookup"><span data-stu-id="5b501-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="5b501-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="5b501-160">section0:key0</span></span>
* <span data-ttu-id="5b501-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="5b501-161">section0:key1</span></span>
* <span data-ttu-id="5b501-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="5b501-162">section1:key0</span></span>
* <span data-ttu-id="5b501-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="5b501-163">section1:key1</span></span>

<span data-ttu-id="5b501-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> a <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> metody jsou k dispozici k izolaci oddíly a podřízené objekty daného oddílu v konfiguračních datech.</span><span class="sxs-lookup"><span data-stu-id="5b501-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="5b501-165">Tyto metody jsou popsané dále v [GetSection GetChildren – a Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="5b501-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="5b501-166">Konvence</span><span class="sxs-lookup"><span data-stu-id="5b501-166">Conventions</span></span>

<span data-ttu-id="5b501-167">Konfigurace zdroje jsou při spuštění aplikace pro čtení v pořadí, že jsou uvedeny příslušné poskytovatele konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5b501-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="5b501-168">Poskytovatelé konfigurace souboru mají možnost znovu načíst konfiguraci je podkladový soubor nastavení se při změně po spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b501-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="5b501-169">Zprostředkovatel konfigurace souboru je popsána dále v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="5b501-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="5b501-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> je k dispozici v aplikaci prvku [Dependency Injection (DI)](xref:fundamentals/dependency-injection) kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5b501-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="5b501-171">Poskytovatelé konfigurace nemůže využít DI, protože není k dispozici při jejich nastavení hostitele.</span><span class="sxs-lookup"><span data-stu-id="5b501-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="5b501-172">Konfigurační klíče použijte následující konvence:</span><span class="sxs-lookup"><span data-stu-id="5b501-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="5b501-173">Klíče jsou malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="5b501-173">Keys are case-insensitive.</span></span> <span data-ttu-id="5b501-174">Například `ConnectionString` a `connectionstring` jsou považovány za ekvivalentní klíče.</span><span class="sxs-lookup"><span data-stu-id="5b501-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="5b501-175">Pokud poskytovateli stejné nebo různé konfigurace je nastavena hodnota pro stejný klíč, poslední hodnotu nastavit na klíč je hodnota.</span><span class="sxs-lookup"><span data-stu-id="5b501-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="5b501-176">Hierarchické klíče</span><span class="sxs-lookup"><span data-stu-id="5b501-176">Hierarchical keys</span></span>
  * <span data-ttu-id="5b501-177">V rámci rozhraní API pro konfiguraci, oddělovač dvojtečka (`:`) funguje na všech platformách.</span><span class="sxs-lookup"><span data-stu-id="5b501-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="5b501-178">V seznamu proměnných prostředí oddělovač dvojtečka nemusí fungovat na všech platformách.</span><span class="sxs-lookup"><span data-stu-id="5b501-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="5b501-179">Dvojitým podtržítkem (`__`) podporuje všechny platformy a je převedena na dvojtečkou.</span><span class="sxs-lookup"><span data-stu-id="5b501-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="5b501-180">Ve službě Azure Key Vault, hierarchické klíče, použijte `--` (dvě pomlčky) jako oddělovač.</span><span class="sxs-lookup"><span data-stu-id="5b501-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="5b501-181">Je nutné zadat kód pomlček nahraďte dvojtečkou tajné klíče jsou načtena do konfigurace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b501-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="5b501-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> Podporuje pole vazby na objekty pomocí indexy pole v konfigurační klíče.</span><span class="sxs-lookup"><span data-stu-id="5b501-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="5b501-183">Pole vazby je popsána v [svázat pole třídy](#bind-an-array-to-a-class) oddílu.</span><span class="sxs-lookup"><span data-stu-id="5b501-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="5b501-184">Hodnoty konfigurace přijmout následující konvence:</span><span class="sxs-lookup"><span data-stu-id="5b501-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="5b501-185">Hodnoty jsou řetězce.</span><span class="sxs-lookup"><span data-stu-id="5b501-185">Values are strings.</span></span>
* <span data-ttu-id="5b501-186">Hodnoty Null nelze uložit v konfiguraci nebo vázány s objekty.</span><span class="sxs-lookup"><span data-stu-id="5b501-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="5b501-187">Zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="5b501-187">Providers</span></span>

<span data-ttu-id="5b501-188">V následující tabulce jsou uvedeny poskytovatelé konfigurace k dispozici pro aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5b501-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="5b501-189">Zprostředkovatel</span><span class="sxs-lookup"><span data-stu-id="5b501-189">Provider</span></span> | <span data-ttu-id="5b501-190">Obsahuje konfiguraci z&hellip;</span><span class="sxs-lookup"><span data-stu-id="5b501-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="5b501-191">[Azure zprostředkovatel konfigurace trezoru klíčů](xref:security/key-vault-configuration) (*zabezpečení* témata)</span><span class="sxs-lookup"><span data-stu-id="5b501-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="5b501-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="5b501-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="5b501-193">Zprostředkovatel konfigurace příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5b501-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="5b501-194">Parametry příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5b501-194">Command-line parameters</span></span> |
| [<span data-ttu-id="5b501-195">Vlastního poskytovatele konfigurace</span><span class="sxs-lookup"><span data-stu-id="5b501-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="5b501-196">Vlastní zdroj</span><span class="sxs-lookup"><span data-stu-id="5b501-196">Custom source</span></span> |
| [<span data-ttu-id="5b501-197">Proměnné prostředí poskytovatele konfigurace</span><span class="sxs-lookup"><span data-stu-id="5b501-197">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="5b501-198">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="5b501-198">Environment variables</span></span> |
| [<span data-ttu-id="5b501-199">Zprostředkovatel konfigurace souboru</span><span class="sxs-lookup"><span data-stu-id="5b501-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="5b501-200">Soubory INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="5b501-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="5b501-201">Poskytovatel konfigurace pro každý soubor klíče</span><span class="sxs-lookup"><span data-stu-id="5b501-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="5b501-202">Adresář souborů</span><span class="sxs-lookup"><span data-stu-id="5b501-202">Directory files</span></span> |
| [<span data-ttu-id="5b501-203">Zprostředkovatel konfigurace paměti</span><span class="sxs-lookup"><span data-stu-id="5b501-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="5b501-204">Kolekce v paměti</span><span class="sxs-lookup"><span data-stu-id="5b501-204">In-memory collections</span></span> |
| <span data-ttu-id="5b501-205">[Tajné klíče uživatelů (tajný klíč správce)](xref:security/app-secrets) (*zabezpečení* témata)</span><span class="sxs-lookup"><span data-stu-id="5b501-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="5b501-206">Soubor v adresáři profilu uživatele</span><span class="sxs-lookup"><span data-stu-id="5b501-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="5b501-207">Zprostředkovatel</span><span class="sxs-lookup"><span data-stu-id="5b501-207">Provider</span></span> | <span data-ttu-id="5b501-208">Obsahuje konfiguraci z&hellip;</span><span class="sxs-lookup"><span data-stu-id="5b501-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="5b501-209">[Azure zprostředkovatel konfigurace trezoru klíčů](xref:security/key-vault-configuration) (*zabezpečení* témata)</span><span class="sxs-lookup"><span data-stu-id="5b501-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="5b501-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="5b501-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="5b501-211">Zprostředkovatel konfigurace příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5b501-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="5b501-212">Parametry příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5b501-212">Command-line parameters</span></span> |
| [<span data-ttu-id="5b501-213">Vlastního poskytovatele konfigurace</span><span class="sxs-lookup"><span data-stu-id="5b501-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="5b501-214">Vlastní zdroj</span><span class="sxs-lookup"><span data-stu-id="5b501-214">Custom source</span></span> |
| [<span data-ttu-id="5b501-215">Proměnné prostředí poskytovatele konfigurace</span><span class="sxs-lookup"><span data-stu-id="5b501-215">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="5b501-216">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="5b501-216">Environment variables</span></span> |
| [<span data-ttu-id="5b501-217">Zprostředkovatel konfigurace souboru</span><span class="sxs-lookup"><span data-stu-id="5b501-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="5b501-218">Soubory INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="5b501-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="5b501-219">Zprostředkovatel konfigurace paměti</span><span class="sxs-lookup"><span data-stu-id="5b501-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="5b501-220">Kolekce v paměti</span><span class="sxs-lookup"><span data-stu-id="5b501-220">In-memory collections</span></span> |
| <span data-ttu-id="5b501-221">[Tajné klíče uživatelů (tajný klíč správce)](xref:security/app-secrets) (*zabezpečení* témata)</span><span class="sxs-lookup"><span data-stu-id="5b501-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="5b501-222">Soubor v adresáři profilu uživatele</span><span class="sxs-lookup"><span data-stu-id="5b501-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="5b501-223">Zprostředkovatel</span><span class="sxs-lookup"><span data-stu-id="5b501-223">Provider</span></span> | <span data-ttu-id="5b501-224">Obsahuje konfiguraci z&hellip;</span><span class="sxs-lookup"><span data-stu-id="5b501-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="5b501-225">Zprostředkovatel konfigurace příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5b501-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="5b501-226">Parametry příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5b501-226">Command-line parameters</span></span> |
| [<span data-ttu-id="5b501-227">Vlastního poskytovatele konfigurace</span><span class="sxs-lookup"><span data-stu-id="5b501-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="5b501-228">Vlastní zdroj</span><span class="sxs-lookup"><span data-stu-id="5b501-228">Custom source</span></span> |
| [<span data-ttu-id="5b501-229">Proměnné prostředí poskytovatele konfigurace</span><span class="sxs-lookup"><span data-stu-id="5b501-229">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="5b501-230">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="5b501-230">Environment variables</span></span> |
| [<span data-ttu-id="5b501-231">Zprostředkovatel konfigurace souboru</span><span class="sxs-lookup"><span data-stu-id="5b501-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="5b501-232">Soubory INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="5b501-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="5b501-233">Zprostředkovatel konfigurace paměti</span><span class="sxs-lookup"><span data-stu-id="5b501-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="5b501-234">Kolekce v paměti</span><span class="sxs-lookup"><span data-stu-id="5b501-234">In-memory collections</span></span> |
| <span data-ttu-id="5b501-235">[Tajné klíče uživatelů (tajný klíč správce)](xref:security/app-secrets) (*zabezpečení* témata)</span><span class="sxs-lookup"><span data-stu-id="5b501-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="5b501-236">Soubor v adresáři profilu uživatele</span><span class="sxs-lookup"><span data-stu-id="5b501-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="5b501-237">Konfigurace zdrojů se čtení v pořadí, že jejich poskytovatelé konfigurace jsou zadány při spuštění.</span><span class="sxs-lookup"><span data-stu-id="5b501-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="5b501-238">Poskytovatelé konfigurace popsané v tomto tématu jsou popsány v abecedním pořadí, ne v pořadí, že váš kód může uspořádat je.</span><span class="sxs-lookup"><span data-stu-id="5b501-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="5b501-239">Pořadí poskytovatelé konfigurace ve vašem kódu tak, aby vyhovoval vašim prioritám pro podkladové zdroje konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5b501-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="5b501-240">Typická posloupnost poskytovatelé konfigurace je:</span><span class="sxs-lookup"><span data-stu-id="5b501-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="5b501-241">Soubory (*appsettings.json*, *appsettings.&lt; Prostředí&gt;.json*, kde `<Environment>` je aktuálním prostředí hostování aplikace)</span><span class="sxs-lookup"><span data-stu-id="5b501-241">Files (*appsettings.json*, *appsettings.&lt;Environment&gt;.json*, where `<Environment>` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="5b501-242">[Tajné klíče uživatelů (tajný klíč správce)](xref:security/app-secrets) (ve vývojovém prostředí jenom)</span><span class="sxs-lookup"><span data-stu-id="5b501-242">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="5b501-243">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="5b501-243">Environment variables</span></span>
1. <span data-ttu-id="5b501-244">Argumenty příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5b501-244">Command-line arguments</span></span>

<span data-ttu-id="5b501-245">Je obvyklé na poslední pozici poskytovatele konfigurace příkazového řádku v sérii poskytovatele, jak povolit argumenty příkazového řádku k přepsání konfigurace nastavená poskytovateli.</span><span class="sxs-lookup"><span data-stu-id="5b501-245">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5b501-246">Toto pořadí poskytovatelů přejde do místa při inicializaci nové <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> s <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="5b501-246">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="5b501-247">Další informace najdete v tématu [webového hostitele: nastavení hostitele](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="5b501-247">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="5b501-248">Volání <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> při vytváření webového hostitele k určení poskytovatelé konfigurace aplikace:</span><span class="sxs-lookup"><span data-stu-id="5b501-248">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="5b501-249">`ConfigureAppConfiguration` *je k dispozici v ASP.NET Core 2.1 nebo novější.*</span><span class="sxs-lookup"><span data-stu-id="5b501-249">`ConfigureAppConfiguration` *is available in ASP.NET Core 2.1 or later.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5b501-250">Toto pořadí poskytovatelů lze vytvořit pro aplikaci (ne hostiteli) s <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> volání a jeho <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> metoda ve `Startup`:</span><span class="sxs-lookup"><span data-stu-id="5b501-250">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

```csharp
public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
            reloadOnChange: true);

    var appAssembly = Assembly.Load(new AssemblyName(env.ApplicationName));

    if (appAssembly != null)
    {
        builder.AddUserSecrets(appAssembly, optional: true);
    }

    builder.AddEnvironmentVariables();

    Configuration = builder.Build();
}

public IConfiguration Configuration { get; }

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IConfiguration>(Configuration);
}
```

<span data-ttu-id="5b501-251">V předchozím příkladu název prostředí (`env.EnvironmentName`) a název aplikace sestavení (`env.ApplicationName`) jsou poskytovány <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="5b501-251">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="5b501-252">Další informace naleznete v tématu <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="5b501-252">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="5b501-253">Zprostředkovatel konfigurace příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5b501-253">Command-line Configuration Provider</span></span>

<span data-ttu-id="5b501-254"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> Načte konfiguraci z páry klíč hodnota pro argument příkazového řádku za běhu.</span><span class="sxs-lookup"><span data-stu-id="5b501-254">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5b501-255">Chcete-li aktivovat příkazového řádku konfigurace, zavolejte <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> rozšiřující metody na instanci <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5b501-255">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5b501-256">`AddCommandLine` je automaticky volána, když inicializujete novou <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> s <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="5b501-256">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="5b501-257">Další informace najdete v tématu [webového hostitele: nastavení hostitele](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="5b501-257">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="5b501-258">`CreateDefaultBuilder` také načítání:</span><span class="sxs-lookup"><span data-stu-id="5b501-258">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="5b501-259">Volitelná konfigurace z *appsettings.json* a *appsettings.&lt; Prostředí&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="5b501-259">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="5b501-260">[Tajné klíče uživatelů (tajný klíč správce)](xref:security/app-secrets) (ve vývojovém prostředí).</span><span class="sxs-lookup"><span data-stu-id="5b501-260">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="5b501-261">Proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="5b501-261">Environment variables.</span></span>

<span data-ttu-id="5b501-262">`CreateDefaultBuilder` poslední přidá poskytovatele konfigurace příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="5b501-262">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="5b501-263">Argumenty příkazového řádku předané za běhu přepsat nastavení ostatní poskytovatelé konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5b501-263">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="5b501-264">`CreateDefaultBuilder` funguje, když je vytvořen hostiteli.</span><span class="sxs-lookup"><span data-stu-id="5b501-264">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="5b501-265">Proto příkazového řádku konfigurace aktivoval `CreateDefaultBuilder` může mít vliv na konfiguraci hostitele.</span><span class="sxs-lookup"><span data-stu-id="5b501-265">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="5b501-266">Při vytváření hostitele ručně a není volání `CreateDefaultBuilder`, zavolejte `AddCommandLine` rozšiřující metody na instanci <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="5b501-266">When building the host manually and not calling `CreateDefaultBuilder`, call the `AddCommandLine` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5b501-267">Chcete-li aktivovat příkazového řádku konfigurace, zavolejte <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> rozšiřující metody na instanci <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5b501-267">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5b501-268">Volání zprostředkovatele poslední Chcete-li povolit argumenty příkazového řádku předané za běhu přepsat konfiguraci nastavením jiných poskytovatelů konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5b501-268">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="5b501-269">Použít konfiguraci <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> s <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> metody:</span><span class="sxs-lookup"><span data-stu-id="5b501-269">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="5b501-270">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="5b501-270">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5b501-271">2.x ukázková aplikace využívá metodu statické pohodlí `CreateDefaultBuilder` k sestavení hostitele, který obsahuje volání <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="5b501-271">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5b501-272">Volání 1.x ukázkové aplikace <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> na <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5b501-272">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="5b501-273">Otevřete příkazový řádek v adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="5b501-273">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="5b501-274">Zadat argument příkazového řádku k `dotnet run` příkazu `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="5b501-274">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="5b501-275">Jakmile je aplikace spuštěná, otevřete prohlížeč na aplikaci na `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5b501-275">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="5b501-276">Podívejte se, že výstup obsahuje pár klíč hodnota pro argument příkazového řádku konfigurace poskytnuté `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="5b501-276">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="5b501-277">Arguments</span><span class="sxs-lookup"><span data-stu-id="5b501-277">Arguments</span></span>

<span data-ttu-id="5b501-278">Hodnota musí následovat znak rovná se (`=`), nebo klíč musí mít předponu (`--` nebo `/`) když hodnota má následující formát mezerou.</span><span class="sxs-lookup"><span data-stu-id="5b501-278">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="5b501-279">Hodnota může mít hodnotu null, pokud se používá znak rovná se (například `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="5b501-279">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="5b501-280">Klíčová předpona</span><span class="sxs-lookup"><span data-stu-id="5b501-280">Key prefix</span></span>               | <span data-ttu-id="5b501-281">Příklad</span><span class="sxs-lookup"><span data-stu-id="5b501-281">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="5b501-282">Žádná předpona.</span><span class="sxs-lookup"><span data-stu-id="5b501-282">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="5b501-283">Dvě pomlčky (`--`)</span><span class="sxs-lookup"><span data-stu-id="5b501-283">Two dashes (`--`)</span></span>        | <span data-ttu-id="5b501-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="5b501-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="5b501-285">Lomítko (`/`)</span><span class="sxs-lookup"><span data-stu-id="5b501-285">Forward slash (`/`)</span></span>      | <span data-ttu-id="5b501-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="5b501-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="5b501-287">V rámci stejného příkazu Nekombinujte argument příkazového řádku páry klíč hodnota, které používají znaménko rovná se s páry klíč hodnota, které používají mezerou.</span><span class="sxs-lookup"><span data-stu-id="5b501-287">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="5b501-288">Příklady příkazů:</span><span class="sxs-lookup"><span data-stu-id="5b501-288">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value --CommandLineKey2=value /CommandLineKey2=value
dotnet run --CommandLineKey1 value /CommandLineKey2 value
dotnet run CommandLineKey1= CommandLineKey2=value
```

### <a name="switch-mappings"></a><span data-ttu-id="5b501-289">Přepnout mapování</span><span class="sxs-lookup"><span data-stu-id="5b501-289">Switch mappings</span></span>

<span data-ttu-id="5b501-290">Mapování přepínači povolit název klíče pro náhradní logiku.</span><span class="sxs-lookup"><span data-stu-id="5b501-290">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="5b501-291">Při ruční sestavení konfigurace s <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, může poskytnout slovník nahrazení přepínači <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metoda.</span><span class="sxs-lookup"><span data-stu-id="5b501-291">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="5b501-292">Při použití přepínače slovníku mapování slovníku se kontroluje u klíč, který odpovídá key určeného tímto argumentem příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="5b501-292">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="5b501-293">Pokud je nalezen příkazového řádku klíč ve slovníku, hodnota slovníku (náhradní klíč) se předá zpět do nastavit pár klíč hodnota v konfiguraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b501-293">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="5b501-294">Mapování přepínač je vyžadován pro libovolného příkazového řádku klíče předponu jeden pomlčka (`-`).</span><span class="sxs-lookup"><span data-stu-id="5b501-294">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="5b501-295">Přepnout mapování slovníku klíčových pravidel:</span><span class="sxs-lookup"><span data-stu-id="5b501-295">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="5b501-296">Přepínače musí začínat pomlčkou (`-`) nebo dvojitou pomlčka (`--`).</span><span class="sxs-lookup"><span data-stu-id="5b501-296">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="5b501-297">Slovník mapování přepínač nesmí obsahovat duplicitní klíče.</span><span class="sxs-lookup"><span data-stu-id="5b501-297">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var switchMappings = new Dictionary<string, string>
            {
                { "-CLKey1", "CommandLineKey1" },
                { "-CLKey2", "CommandLineKey2" }
            };

        var config = new ConfigurationBuilder()
            .AddCommandLine(args, switchMappings)
            .Build();

        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="5b501-298">Jak je znázorněno v předchozím příkladu volání `CreateDefaultBuilder` by neměla předat argumenty při mapování přepínače se používají.</span><span class="sxs-lookup"><span data-stu-id="5b501-298">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="5b501-299">`CreateDefaultBuilder` metody `AddCommandLine` volání neobsahuje mapované přepínače a neexistuje žádný způsob předat slovníku mapování přepínač k `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5b501-299">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="5b501-300">Pokud argumenty obsahují mapované přepínače a jsou předány `CreateDefaultBuilder`, jeho `AddCommandLine` poskytovatele dojde k selhání inicializace s <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="5b501-300">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="5b501-301">Řešení se předat argumenty, které mají `CreateDefaultBuilder` , ale místo toho povolit `ConfigurationBuilder` metody `AddCommandLine` metodu ke zpracování argumentů a slovníku mapování přepínače.</span><span class="sxs-lookup"><span data-stu-id="5b501-301">The solution is not to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    var switchMappings = new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    var config = new ConfigurationBuilder()
        .AddCommandLine(args, switchMappings)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .UseStartup<Startup>()
        .Start();

    using (host)
    {
        Console.ReadLine();
    }
}
```

::: moniker-end

<span data-ttu-id="5b501-302">Po vytvoření slovníku mapování přepínač obsahuje data zobrazená v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="5b501-302">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="5b501-303">Key</span><span class="sxs-lookup"><span data-stu-id="5b501-303">Key</span></span>       | <span data-ttu-id="5b501-304">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5b501-304">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="5b501-305">Pokud při spuštění aplikace se používají klíče mapované na přepínač, konfigurace přijímá konfigurační hodnoty klíče zadané ve slovníku:</span><span class="sxs-lookup"><span data-stu-id="5b501-305">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="5b501-306">Po spuštění předchozího příkazu se konfigurace obsahuje hodnoty uvedené v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="5b501-306">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="5b501-307">Key</span><span class="sxs-lookup"><span data-stu-id="5b501-307">Key</span></span>               | <span data-ttu-id="5b501-308">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5b501-308">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="5b501-309">Zprostředkovatel konfigurace proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="5b501-309">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="5b501-310"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> Načte konfiguraci z prostředí proměnné páry klíč hodnota v době běhu.</span><span class="sxs-lookup"><span data-stu-id="5b501-310">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="5b501-311">Chcete-li aktivovat konfigurace proměnných prostředí, zavolejte <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> rozšiřující metody na instanci <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5b501-311">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5b501-312">Při práci s hierarchické klíče v seznamu proměnných prostředí, oddělovač dvojtečka (`:`) nemusí fungovat na všech platformách.</span><span class="sxs-lookup"><span data-stu-id="5b501-312">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="5b501-313">Dvojitým podtržítkem (`__`) podporuje všechny platformy a nahrazuje dvojtečkou.</span><span class="sxs-lookup"><span data-stu-id="5b501-313">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="5b501-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) umožňuje nastavit proměnné prostředí na webu Azure Portal, můžete přepsat konfiguraci aplikace pomocí zprostředkovatele konfigurace proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="5b501-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="5b501-315">Další informace najdete v tématu [aplikace Azure: Konfigurace aplikace přepsat pomocí webu Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="5b501-315">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5b501-316">`AddEnvironmentVariables` je automaticky volána, když inicializujete novou <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> s <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="5b501-316">`AddEnvironmentVariables` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="5b501-317">Další informace najdete v tématu [webového hostitele: nastavení hostitele](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="5b501-317">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="5b501-318">`CreateDefaultBuilder` také načítání:</span><span class="sxs-lookup"><span data-stu-id="5b501-318">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="5b501-319">Volitelná konfigurace z *appsettings.json* a *appsettings.&lt; Prostředí&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="5b501-319">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="5b501-320">[Tajné klíče uživatelů (tajný klíč správce)](xref:security/app-secrets) (ve vývojovém prostředí).</span><span class="sxs-lookup"><span data-stu-id="5b501-320">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="5b501-321">Argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="5b501-321">Command-line arguments.</span></span>

<span data-ttu-id="5b501-322">Poskytovatel konfigurace proměnných prostředí je volána po navázání konfigurace z tajných kódů uživatelů a *appsettings* soubory.</span><span class="sxs-lookup"><span data-stu-id="5b501-322">The Environment Variable Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="5b501-323">Volání zprostředkovatele na této pozici umožňuje proměnné prostředí načteny za běhu přepsat konfiguraci nastavil tajných kódů uživatelů a *appsettings* soubory.</span><span class="sxs-lookup"><span data-stu-id="5b501-323">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="5b501-324">Můžete také přímo zavolat `AddEnvironmentVariables` rozšiřující metody na instanci <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="5b501-324">You can also directly call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5b501-325">Použít konfiguraci <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> s `UseConfiguration` metody:</span><span class="sxs-lookup"><span data-stu-id="5b501-325">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="5b501-326">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="5b501-326">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5b501-327">2.x ukázková aplikace využívá metodu statické pohodlí `CreateDefaultBuilder` k sestavení hostitele, který obsahuje volání `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="5b501-327">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5b501-328">Volání 1.x ukázkové aplikace `AddEnvironmentVariables` na `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5b501-328">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="5b501-329">Spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b501-329">Run the sample app.</span></span> <span data-ttu-id="5b501-330">Otevřete prohlížeč na aplikaci na `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5b501-330">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="5b501-331">Podívejte se, že výstup obsahuje pár klíč hodnota pro proměnnou prostředí `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="5b501-331">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="5b501-332">Hodnota odpovídá prostředí, ve kterém je aplikace spuštěna, obvykle `Development` při místním spuštění.</span><span class="sxs-lookup"><span data-stu-id="5b501-332">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="5b501-333">Udržovat seznam proměnných prostředí vykreslen metodou aplikace krátký, filtry aplikace proměnných prostředí až po ty spustit následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="5b501-333">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="5b501-334">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="5b501-334">ASPNETCORE_</span></span>
* <span data-ttu-id="5b501-335">adresy URL</span><span class="sxs-lookup"><span data-stu-id="5b501-335">urls</span></span>
* <span data-ttu-id="5b501-336">Protokolování</span><span class="sxs-lookup"><span data-stu-id="5b501-336">Logging</span></span>
* <span data-ttu-id="5b501-337">PROSTŘEDÍ</span><span class="sxs-lookup"><span data-stu-id="5b501-337">ENVIRONMENT</span></span>
* <span data-ttu-id="5b501-338">contentRoot</span><span class="sxs-lookup"><span data-stu-id="5b501-338">contentRoot</span></span>
* <span data-ttu-id="5b501-339">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="5b501-339">AllowedHosts</span></span>
* <span data-ttu-id="5b501-340">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="5b501-340">applicationName</span></span>
* <span data-ttu-id="5b501-341">příkazový řádek</span><span class="sxs-lookup"><span data-stu-id="5b501-341">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5b501-342">Pokud chcete zpřístupnit všechny proměnné prostředí k dispozici pro aplikace, změnit `FilteredConfiguration` v *Pages/Index.cshtml.cs* takto:</span><span class="sxs-lookup"><span data-stu-id="5b501-342">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5b501-343">Pokud chcete zpřístupnit všechny proměnné prostředí k dispozici pro aplikace, změnit `FilteredConfiguration` v *Controllers/HomeController.cs* takto:</span><span class="sxs-lookup"><span data-stu-id="5b501-343">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="5b501-344">Předpony adres</span><span class="sxs-lookup"><span data-stu-id="5b501-344">Prefixes</span></span>

<span data-ttu-id="5b501-345">Proměnné prostředí načteny do konfiguraci aplikace, jsou filtrovány, když zadáte předponu `AddEnvironmentVariables` metody.</span><span class="sxs-lookup"><span data-stu-id="5b501-345">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="5b501-346">Například k proměnným prostředí filtr na této předponě `CUSTOM_`, zadat předponu pro zprostředkovatele konfigurace:</span><span class="sxs-lookup"><span data-stu-id="5b501-346">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="5b501-347">Předpona, která se odstraní při vytváření konfigurace páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="5b501-347">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5b501-348">Metoda statické pohodlí `CreateDefaultBuilder` vytvoří <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> stanovit hostiteli aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b501-348">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="5b501-349">Když `WebHostBuilder` je vytvořen, zjistí jeho konfigurace hostitele v proměnných prostředí s předponou `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="5b501-349">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="5b501-350">**Připojovací řetězec předpony**</span><span class="sxs-lookup"><span data-stu-id="5b501-350">**Connection string prefixes**</span></span>

<span data-ttu-id="5b501-351">Rozhraní API konfigurace má zvláštní zpracování pravidel pro čtyři připojovací řetězec proměnné prostředí používané při konfiguraci Azure připojovací řetězce pro prostředí app.</span><span class="sxs-lookup"><span data-stu-id="5b501-351">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="5b501-352">Proměnné prostředí s předponami uvedené v tabulce se načtou do aplikace, pokud není zadána žádná předpona k `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="5b501-352">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="5b501-353">Předpona řetězce připojení</span><span class="sxs-lookup"><span data-stu-id="5b501-353">Connection string prefix</span></span> | <span data-ttu-id="5b501-354">Zprostředkovatel</span><span class="sxs-lookup"><span data-stu-id="5b501-354">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="5b501-355">Vlastní zprostředkovatel</span><span class="sxs-lookup"><span data-stu-id="5b501-355">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="5b501-356">MySQL</span><span class="sxs-lookup"><span data-stu-id="5b501-356">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="5b501-357">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="5b501-357">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="5b501-358">SQL Server</span><span class="sxs-lookup"><span data-stu-id="5b501-358">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="5b501-359">Proměnné prostředí je při zjištění a načtena do konfigurace s žádným z čtyři předpony, které jsou uvedené v tabulce:</span><span class="sxs-lookup"><span data-stu-id="5b501-359">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="5b501-360">Konfigurační klíč je vytvořen odebráním předponu proměnné prostředí a přidání konfiguračního klíče oddílu (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="5b501-360">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="5b501-361">Je vytvořen nový pár klíč hodnota konfigurace, který představuje poskytovatele připojení databáze (s výjimkou `CUSTOMCONNSTR_`, který nemá stanoveného zprostředkovatele).</span><span class="sxs-lookup"><span data-stu-id="5b501-361">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="5b501-362">Klíč proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="5b501-362">Environment variable key</span></span> | <span data-ttu-id="5b501-363">Převedený konfigurační klíč</span><span class="sxs-lookup"><span data-stu-id="5b501-363">Converted configuration key</span></span> | <span data-ttu-id="5b501-364">Položka konfigurace zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="5b501-364">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="5b501-365">Položky konfigurace nebyl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="5b501-365">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="5b501-366">Klíč: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="5b501-366">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="5b501-367">Hodnota: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="5b501-367">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="5b501-368">Klíč: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="5b501-368">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="5b501-369">Hodnota: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="5b501-369">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="5b501-370">Klíč: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="5b501-370">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="5b501-371">Hodnota: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="5b501-371">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="5b501-372">Zprostředkovatel konfigurace souboru</span><span class="sxs-lookup"><span data-stu-id="5b501-372">File Configuration Provider</span></span>

<span data-ttu-id="5b501-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> je základní třída pro načítání konfigurace ze systému souborů.</span><span class="sxs-lookup"><span data-stu-id="5b501-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="5b501-374">Následující zprostředkovatele konfigurace jsou vyhrazené pro určité typy souborů:</span><span class="sxs-lookup"><span data-stu-id="5b501-374">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="5b501-375">Zprostředkovatel konfigurace INI</span><span class="sxs-lookup"><span data-stu-id="5b501-375">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="5b501-376">Zprostředkovatel konfigurace JSON</span><span class="sxs-lookup"><span data-stu-id="5b501-376">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="5b501-377">Zprostředkovatel konfigurace XML</span><span class="sxs-lookup"><span data-stu-id="5b501-377">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="5b501-378">Zprostředkovatel konfigurace INI</span><span class="sxs-lookup"><span data-stu-id="5b501-378">INI Configuration Provider</span></span>

<span data-ttu-id="5b501-379"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> Načte konfiguraci z páry klíč hodnota soubor INI za běhu.</span><span class="sxs-lookup"><span data-stu-id="5b501-379">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="5b501-380">Chcete-li aktivovat konfigurační soubor INI, zavolejte <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> rozšiřující metody na instanci <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5b501-380">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5b501-381">Dvojtečka je možné k jako oddělovač oddílu v konfiguraci souboru INI.</span><span class="sxs-lookup"><span data-stu-id="5b501-381">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="5b501-382">Přetížení povolit zadáním:</span><span class="sxs-lookup"><span data-stu-id="5b501-382">Overloads permit specifying:</span></span>

* <span data-ttu-id="5b501-383">Určuje, zda soubor je volitelné.</span><span class="sxs-lookup"><span data-stu-id="5b501-383">Whether the file is optional.</span></span>
* <span data-ttu-id="5b501-384">Určuje, zda konfigurace opětovném načtení nástroje Pokud se soubor změní.</span><span class="sxs-lookup"><span data-stu-id="5b501-384">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="5b501-385"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Používá pro přístup k souboru.</span><span class="sxs-lookup"><span data-stu-id="5b501-385">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5b501-386">Při volání metody `CreateDefaultBuilder`, volání `UseConfiguration` s konfigurací:</span><span class="sxs-lookup"><span data-stu-id="5b501-386">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddIniFile("config.ini", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="5b501-387">Při vytváření <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> volat přímo, `UseConfiguration` s konfigurací:</span><span class="sxs-lookup"><span data-stu-id="5b501-387">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5b501-388">Použít konfiguraci <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> s `UseConfiguration` metody:</span><span class="sxs-lookup"><span data-stu-id="5b501-388">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddIniFile("config.ini", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="5b501-389">Obecný příklad konfiguračního souboru INI:</span><span class="sxs-lookup"><span data-stu-id="5b501-389">A generic example of an INI configuration file:</span></span>

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

<span data-ttu-id="5b501-390">Předchozí konfigurační soubor načte následujících klíčů s `value`:</span><span class="sxs-lookup"><span data-stu-id="5b501-390">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="5b501-391">section0:key0</span><span class="sxs-lookup"><span data-stu-id="5b501-391">section0:key0</span></span>
* <span data-ttu-id="5b501-392">section0:key1</span><span class="sxs-lookup"><span data-stu-id="5b501-392">section0:key1</span></span>
* <span data-ttu-id="5b501-393">section1:subsection:Key</span><span class="sxs-lookup"><span data-stu-id="5b501-393">section1:subsection:key</span></span>
* <span data-ttu-id="5b501-394">section2:subsection0:Key</span><span class="sxs-lookup"><span data-stu-id="5b501-394">section2:subsection0:key</span></span>
* <span data-ttu-id="5b501-395">section2:subsection1:Key</span><span class="sxs-lookup"><span data-stu-id="5b501-395">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="5b501-396">Zprostředkovatel konfigurace JSON</span><span class="sxs-lookup"><span data-stu-id="5b501-396">JSON Configuration Provider</span></span>

<span data-ttu-id="5b501-397"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> Načte konfiguraci z páry klíč hodnota JSON soubor za běhu.</span><span class="sxs-lookup"><span data-stu-id="5b501-397">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="5b501-398">Chcete-li aktivovat konfigurační soubor JSON, zavolejte <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> rozšiřující metody na instanci <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5b501-398">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5b501-399">Přetížení povolit zadáním:</span><span class="sxs-lookup"><span data-stu-id="5b501-399">Overloads permit specifying:</span></span>

* <span data-ttu-id="5b501-400">Určuje, zda soubor je volitelné.</span><span class="sxs-lookup"><span data-stu-id="5b501-400">Whether the file is optional.</span></span>
* <span data-ttu-id="5b501-401">Určuje, zda konfigurace opětovném načtení nástroje Pokud se soubor změní.</span><span class="sxs-lookup"><span data-stu-id="5b501-401">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="5b501-402"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Používá pro přístup k souboru.</span><span class="sxs-lookup"><span data-stu-id="5b501-402">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5b501-403">`AddJsonFile` je automaticky volána dvakrát při inicializaci nové <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> s <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="5b501-403">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="5b501-404">Načtení konfigurace z volání metody:</span><span class="sxs-lookup"><span data-stu-id="5b501-404">The method is called to load configuration from:</span></span>

* <span data-ttu-id="5b501-405">*appSettings.JSON* &ndash; tento soubor je nejprve pro čtení.</span><span class="sxs-lookup"><span data-stu-id="5b501-405">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="5b501-406">Prostředí verze souboru mohou přepsat hodnoty poskytnuté *appsettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="5b501-406">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="5b501-407">*appSettings. &lt;Prostředí&gt;.json* &ndash; načtení na základě prostředí verze souboru [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="5b501-407">*appsettings.&lt;Environment&gt;.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="5b501-408">Další informace najdete v tématu [webového hostitele: nastavení hostitele](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="5b501-408">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="5b501-409">`CreateDefaultBuilder` také načítání:</span><span class="sxs-lookup"><span data-stu-id="5b501-409">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="5b501-410">Proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="5b501-410">Environment variables.</span></span>
* <span data-ttu-id="5b501-411">[Tajné klíče uživatelů (tajný klíč správce)](xref:security/app-secrets) (ve vývojovém prostředí).</span><span class="sxs-lookup"><span data-stu-id="5b501-411">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="5b501-412">Argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="5b501-412">Command-line arguments.</span></span>

<span data-ttu-id="5b501-413">Zprostředkovatel konfigurace JSON je nejprve navázat.</span><span class="sxs-lookup"><span data-stu-id="5b501-413">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="5b501-414">Proto tajné klíče uživatelů, proměnné a argumenty příkazového řádku přepsat nastavením konfigurace *appsettings* soubory.</span><span class="sxs-lookup"><span data-stu-id="5b501-414">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="5b501-415">Můžete také přímo zavolat `AddJsonFile` rozšiřující metody na instanci <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5b501-415">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5b501-416">Při volání metody `CreateDefaultBuilder`, volání `UseConfiguration` s konfigurací:</span><span class="sxs-lookup"><span data-stu-id="5b501-416">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("config.json", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="5b501-417">Při vytváření <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> volat přímo, `UseConfiguration` s konfigurací:</span><span class="sxs-lookup"><span data-stu-id="5b501-417">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5b501-418">Použít konfiguraci <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> s `UseConfiguration` metody:</span><span class="sxs-lookup"><span data-stu-id="5b501-418">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("config.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="5b501-419">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="5b501-419">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5b501-420">2.x ukázková aplikace využívá metodu statické pohodlí `CreateDefaultBuilder` k sestavení hostitele, která zahrnuje dvě volání `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="5b501-420">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="5b501-421">Konfigurace je načtena z *appsettings.json* a *appsettings.&lt; Prostředí&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="5b501-421">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5b501-422">Volání 1.x ukázkové aplikace `AddJsonFile` dvakrát na `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5b501-422">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="5b501-423">Konfigurace je načtena z *appsettings.json* a *appsettings.&lt; Prostředí&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="5b501-423">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="5b501-424">Spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b501-424">Run the sample app.</span></span> <span data-ttu-id="5b501-425">Otevřete prohlížeč na aplikaci na `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5b501-425">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="5b501-426">Podívejte se, že výstup obsahuje páry klíč hodnota u konfigurace uvedené v tabulce v závislosti na prostředí.</span><span class="sxs-lookup"><span data-stu-id="5b501-426">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="5b501-427">Protokolování konfigurační klíče pomocí dvojtečky (`:`) jako oddělovačem hierarchický.</span><span class="sxs-lookup"><span data-stu-id="5b501-427">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="5b501-428">Key</span><span class="sxs-lookup"><span data-stu-id="5b501-428">Key</span></span>                        | <span data-ttu-id="5b501-429">Hodnota vývoj</span><span class="sxs-lookup"><span data-stu-id="5b501-429">Development Value</span></span> | <span data-ttu-id="5b501-430">Hodnota produkce</span><span class="sxs-lookup"><span data-stu-id="5b501-430">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="5b501-431">Protokolování: LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="5b501-431">Logging:LogLevel:System</span></span>    | <span data-ttu-id="5b501-432">Informace o</span><span class="sxs-lookup"><span data-stu-id="5b501-432">Information</span></span>       | <span data-ttu-id="5b501-433">Informace o</span><span class="sxs-lookup"><span data-stu-id="5b501-433">Information</span></span>      |
| <span data-ttu-id="5b501-434">Protokolování: LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="5b501-434">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="5b501-435">Informace o</span><span class="sxs-lookup"><span data-stu-id="5b501-435">Information</span></span>       | <span data-ttu-id="5b501-436">Informace o</span><span class="sxs-lookup"><span data-stu-id="5b501-436">Information</span></span>      |
| <span data-ttu-id="5b501-437">Protokolování: LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="5b501-437">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="5b501-438">Ladit</span><span class="sxs-lookup"><span data-stu-id="5b501-438">Debug</span></span>             | <span data-ttu-id="5b501-439">Chyba</span><span class="sxs-lookup"><span data-stu-id="5b501-439">Error</span></span>            |
| <span data-ttu-id="5b501-440">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="5b501-440">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="5b501-441">Zprostředkovatel konfigurace XML</span><span class="sxs-lookup"><span data-stu-id="5b501-441">XML Configuration Provider</span></span>

<span data-ttu-id="5b501-442"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> Načte konfiguraci z dvojice klíč hodnota XML soubor za běhu.</span><span class="sxs-lookup"><span data-stu-id="5b501-442">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="5b501-443">Chcete-li aktivovat konfigurační soubor XML, zavolejte <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> rozšiřující metody na instanci <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5b501-443">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5b501-444">Přetížení povolit zadáním:</span><span class="sxs-lookup"><span data-stu-id="5b501-444">Overloads permit specifying:</span></span>

* <span data-ttu-id="5b501-445">Určuje, zda soubor je volitelné.</span><span class="sxs-lookup"><span data-stu-id="5b501-445">Whether the file is optional.</span></span>
* <span data-ttu-id="5b501-446">Určuje, zda konfigurace opětovném načtení nástroje Pokud se soubor změní.</span><span class="sxs-lookup"><span data-stu-id="5b501-446">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="5b501-447"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Používá pro přístup k souboru.</span><span class="sxs-lookup"><span data-stu-id="5b501-447">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="5b501-448">Kořenový uzel konfiguračního souboru se ignoruje při vytváření konfigurace páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="5b501-448">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="5b501-449">V souboru zadejte dokumentu typ definice (DTD) nebo obor názvů.</span><span class="sxs-lookup"><span data-stu-id="5b501-449">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5b501-450">Při volání metody `CreateDefaultBuilder`, volání `UseConfiguration` s konfigurací:</span><span class="sxs-lookup"><span data-stu-id="5b501-450">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="5b501-451">Při vytváření <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> volat přímo, `UseConfiguration` s konfigurací:</span><span class="sxs-lookup"><span data-stu-id="5b501-451">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5b501-452">Použít konfiguraci <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> s `UseConfiguration` metody:</span><span class="sxs-lookup"><span data-stu-id="5b501-452">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="5b501-453">Soubory XML konfigurace můžete použít názvy různých elementů pro opakující se části:</span><span class="sxs-lookup"><span data-stu-id="5b501-453">XML configuration files can use distinct element names for repeating sections:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

<span data-ttu-id="5b501-454">Předchozí konfigurační soubor načte následujících klíčů s `value`:</span><span class="sxs-lookup"><span data-stu-id="5b501-454">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="5b501-455">section0:key0</span><span class="sxs-lookup"><span data-stu-id="5b501-455">section0:key0</span></span>
* <span data-ttu-id="5b501-456">section0:key1</span><span class="sxs-lookup"><span data-stu-id="5b501-456">section0:key1</span></span>
* <span data-ttu-id="5b501-457">section1:key0</span><span class="sxs-lookup"><span data-stu-id="5b501-457">section1:key0</span></span>
* <span data-ttu-id="5b501-458">section1:key1</span><span class="sxs-lookup"><span data-stu-id="5b501-458">section1:key1</span></span>

<span data-ttu-id="5b501-459">Opakující se elementy, které používají stejný název elementu fungovat, pokud `name` atribut se používá k rozlišení prvků:</span><span class="sxs-lookup"><span data-stu-id="5b501-459">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

<span data-ttu-id="5b501-460">Předchozí konfigurační soubor načte následujících klíčů s `value`:</span><span class="sxs-lookup"><span data-stu-id="5b501-460">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="5b501-461">část: section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="5b501-461">section:section0:key:key0</span></span>
* <span data-ttu-id="5b501-462">část: section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="5b501-462">section:section0:key:key1</span></span>
* <span data-ttu-id="5b501-463">část: section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="5b501-463">section:section1:key:key0</span></span>
* <span data-ttu-id="5b501-464">část: section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="5b501-464">section:section1:key:key1</span></span>

<span data-ttu-id="5b501-465">Atributy lze použít k zadání hodnoty:</span><span class="sxs-lookup"><span data-stu-id="5b501-465">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="5b501-466">Předchozí konfigurační soubor načte následujících klíčů s `value`:</span><span class="sxs-lookup"><span data-stu-id="5b501-466">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="5b501-467">atribut Key:</span><span class="sxs-lookup"><span data-stu-id="5b501-467">key:attribute</span></span>
* <span data-ttu-id="5b501-468">část: klíč: atribut</span><span class="sxs-lookup"><span data-stu-id="5b501-468">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="5b501-469">Poskytovatel konfigurace pro každý soubor klíče</span><span class="sxs-lookup"><span data-stu-id="5b501-469">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="5b501-470"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> Používá souborů v adresáři jako páry klíč hodnota konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5b501-470">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="5b501-471">Klíč je název souboru.</span><span class="sxs-lookup"><span data-stu-id="5b501-471">The key is the file name.</span></span> <span data-ttu-id="5b501-472">Hodnota obsahuje obsah souboru.</span><span class="sxs-lookup"><span data-stu-id="5b501-472">The value contains the file's contents.</span></span> <span data-ttu-id="5b501-473">Konfiguraci poskytovatele za soubor klíče se používá ve scénářích hostování Dockeru.</span><span class="sxs-lookup"><span data-stu-id="5b501-473">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="5b501-474">Chcete-li aktivovat konfigurace na soubor klíče, zavolejte <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> rozšiřující metody na instanci <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5b501-474">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="5b501-475">`directoryPath` k souborům musí být absolutní cesta.</span><span class="sxs-lookup"><span data-stu-id="5b501-475">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="5b501-476">Přetížení povolit zadáním:</span><span class="sxs-lookup"><span data-stu-id="5b501-476">Overloads permit specifying:</span></span>

* <span data-ttu-id="5b501-477">`Action<KeyPerFileConfigurationSource>` Delegáta, který nastaví jako zdroj.</span><span class="sxs-lookup"><span data-stu-id="5b501-477">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="5b501-478">Určuje, zda daný adresář je volitelné a cestu k adresáři.</span><span class="sxs-lookup"><span data-stu-id="5b501-478">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="5b501-479">Při volání metody `CreateDefaultBuilder`, volání `UseConfiguration` s konfigurací:</span><span class="sxs-lookup"><span data-stu-id="5b501-479">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
        var config = new ConfigurationBuilder()
            .AddKeyPerFile(directoryPath: path, optional: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="5b501-480">Při vytváření <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> volat přímo, `UseConfiguration` s konfigurací:</span><span class="sxs-lookup"><span data-stu-id="5b501-480">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

```csharp
var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
var config = new ConfigurationBuilder()
    .AddKeyPerFile(directoryPath: path, optional: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

::: moniker-end

## <a name="memory-configuration-provider"></a><span data-ttu-id="5b501-481">Zprostředkovatel konfigurace paměti</span><span class="sxs-lookup"><span data-stu-id="5b501-481">Memory Configuration Provider</span></span>

<span data-ttu-id="5b501-482"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> Používá kolekci v paměti jako páry klíč hodnota konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5b501-482">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="5b501-483">Chcete-li aktivovat konfiguraci kolekce v paměti, zavolejte <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> rozšiřující metody na instanci <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5b501-483">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="5b501-484">Poskytovatel konfigurace mohou být inicializovány pomocí `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="5b501-484">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5b501-485">Při volání metody `CreateDefaultBuilder`, volání `UseConfiguration` s konfigurací:</span><span class="sxs-lookup"><span data-stu-id="5b501-485">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

        var config = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="5b501-486">Při vytváření <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> volat přímo, `UseConfiguration` s konfigurací:</span><span class="sxs-lookup"><span data-stu-id="5b501-486">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5b501-487">Použít konfiguraci <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> s `UseConfiguration` metody:</span><span class="sxs-lookup"><span data-stu-id="5b501-487">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

```csharp
var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

var config = new ConfigurationBuilder()
    .AddInMemoryCollection(dict)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="getvalue"></a><span data-ttu-id="5b501-488">GetValue</span><span class="sxs-lookup"><span data-stu-id="5b501-488">GetValue</span></span>

<span data-ttu-id="5b501-489">[ConfigurationBinder.GetValue&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrahuje hodnotu z konfigurace se zadaným klíčem a převede ho na zadaný typ.</span><span class="sxs-lookup"><span data-stu-id="5b501-489">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="5b501-490">Přetížení umožňuje zadat výchozí hodnotu, pokud není nalezen klíč.</span><span class="sxs-lookup"><span data-stu-id="5b501-490">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="5b501-491">Následující příklad získá řetězcovou hodnotu z konfigurace s klíčem `NumberKey`, zadá hodnotu jako `int`a uloží hodnotu do proměnné `intValue`.</span><span class="sxs-lookup"><span data-stu-id="5b501-491">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="5b501-492">Pokud `NumberKey` nebyl nalezen v konfigurační klíče `intValue` přijme výchozí hodnotu `99`:</span><span class="sxs-lookup"><span data-stu-id="5b501-492">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="5b501-493">GetSection, GetChildren – a existuje</span><span class="sxs-lookup"><span data-stu-id="5b501-493">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="5b501-494">Příklady, které následují vezměte v úvahu následující soubor JSON.</span><span class="sxs-lookup"><span data-stu-id="5b501-494">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="5b501-495">Čtyři kódy se nacházejí ve dvou částech, z nichž jeden obsahuje dvojice pododdíly:</span><span class="sxs-lookup"><span data-stu-id="5b501-495">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

<span data-ttu-id="5b501-496">Přečtení souboru do konfigurace následujících jedinečné klíče hierarchické jsou vytvořeny pro uchování hodnoty konfigurace:</span><span class="sxs-lookup"><span data-stu-id="5b501-496">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="5b501-497">section0:key0</span><span class="sxs-lookup"><span data-stu-id="5b501-497">section0:key0</span></span>
* <span data-ttu-id="5b501-498">section0:key1</span><span class="sxs-lookup"><span data-stu-id="5b501-498">section0:key1</span></span>
* <span data-ttu-id="5b501-499">section1:key0</span><span class="sxs-lookup"><span data-stu-id="5b501-499">section1:key0</span></span>
* <span data-ttu-id="5b501-500">section1:key1</span><span class="sxs-lookup"><span data-stu-id="5b501-500">section1:key1</span></span>
* <span data-ttu-id="5b501-501">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="5b501-501">section2:subsection0:key0</span></span>
* <span data-ttu-id="5b501-502">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="5b501-502">section2:subsection0:key1</span></span>
* <span data-ttu-id="5b501-503">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="5b501-503">section2:subsection1:key0</span></span>
* <span data-ttu-id="5b501-504">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="5b501-504">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="5b501-505">GetSection</span><span class="sxs-lookup"><span data-stu-id="5b501-505">GetSection</span></span>

<span data-ttu-id="5b501-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrahuje dílčí část konfigurace s klíčem zadaného dílčí část.</span><span class="sxs-lookup"><span data-stu-id="5b501-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="5b501-507">Se vraťte <xref:Microsoft.Extensions.Configuration.IConfigurationSection> obsahující pouze páry klíč hodnota v `section1`, volání `GetSection` a zadat název oddílu:</span><span class="sxs-lookup"><span data-stu-id="5b501-507">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="5b501-508">Podobně a získat tak hodnoty pro klíče v `section2:subsection0`, volání `GetSection` a zadejte cestu k oddílu:</span><span class="sxs-lookup"><span data-stu-id="5b501-508">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="5b501-509">`GetSection` nikdy nevrátí `null`.</span><span class="sxs-lookup"><span data-stu-id="5b501-509">`GetSection` never returns `null`.</span></span> <span data-ttu-id="5b501-510">Pokud není nalezen odpovídající části, prázdná `IConfigurationSection` je vrácena.</span><span class="sxs-lookup"><span data-stu-id="5b501-510">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="5b501-511">GetChildren –</span><span class="sxs-lookup"><span data-stu-id="5b501-511">GetChildren</span></span>

<span data-ttu-id="5b501-512">Volání [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) na `section2` získá `IEnumerable<IConfigurationSection>` , který obsahuje:</span><span class="sxs-lookup"><span data-stu-id="5b501-512">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="5b501-513">Existuje</span><span class="sxs-lookup"><span data-stu-id="5b501-513">Exists</span></span>

<span data-ttu-id="5b501-514">Použití [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) k určení, jestli existuje konfigurační oddíl:</span><span class="sxs-lookup"><span data-stu-id="5b501-514">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="5b501-515">Zadaný ukázková data `sectionExists` je `false` protože není k dispozici `section2:subsection2` části v konfiguračních datech.</span><span class="sxs-lookup"><span data-stu-id="5b501-515">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="5b501-516">Vytvoření vazby na třídu</span><span class="sxs-lookup"><span data-stu-id="5b501-516">Bind to a class</span></span>

<span data-ttu-id="5b501-517">Konfigurace může být vázaný na třídy, které představující skupiny související nastavení použití *možnosti vzor*.</span><span class="sxs-lookup"><span data-stu-id="5b501-517">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="5b501-518">Další informace naleznete v tématu <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="5b501-518">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="5b501-519">Konfigurační hodnoty jsou vrácené jako řetězce, ale volání <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> umožňuje konstrukci [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objekty.</span><span class="sxs-lookup"><span data-stu-id="5b501-519">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="5b501-520">Obsahuje ukázkovou aplikaci `Starship` modelu (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="5b501-520">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5b501-521">`starship` Část *starship.json* soubor vytvoří konfiguraci při ukázková aplikace používá zprostředkovatele konfigurace JSON načíst konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="5b501-521">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="5b501-522">Následující páry klíč hodnota konfigurace jsou vytvořeny:</span><span class="sxs-lookup"><span data-stu-id="5b501-522">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="5b501-523">Key</span><span class="sxs-lookup"><span data-stu-id="5b501-523">Key</span></span>                   | <span data-ttu-id="5b501-524">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5b501-524">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="5b501-525">starship: name</span><span class="sxs-lookup"><span data-stu-id="5b501-525">starship:name</span></span>         | <span data-ttu-id="5b501-526">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="5b501-526">USS Enterprise</span></span>                                    |
| <span data-ttu-id="5b501-527">starship: registru</span><span class="sxs-lookup"><span data-stu-id="5b501-527">starship:registry</span></span>     | <span data-ttu-id="5b501-528">PADĚLKY 1701</span><span class="sxs-lookup"><span data-stu-id="5b501-528">NCC-1701</span></span>                                          |
| <span data-ttu-id="5b501-529">starship: Třída</span><span class="sxs-lookup"><span data-stu-id="5b501-529">starship:class</span></span>        | <span data-ttu-id="5b501-530">Vytvoření</span><span class="sxs-lookup"><span data-stu-id="5b501-530">Constitution</span></span>                                      |
| <span data-ttu-id="5b501-531">starship: délka</span><span class="sxs-lookup"><span data-stu-id="5b501-531">starship:length</span></span>       | <span data-ttu-id="5b501-532">304.8</span><span class="sxs-lookup"><span data-stu-id="5b501-532">304.8</span></span>                                             |
| <span data-ttu-id="5b501-533">starship: stává</span><span class="sxs-lookup"><span data-stu-id="5b501-533">starship:commissioned</span></span> | <span data-ttu-id="5b501-534">False</span><span class="sxs-lookup"><span data-stu-id="5b501-534">False</span></span>                                             |
| <span data-ttu-id="5b501-535">Ochranné známky</span><span class="sxs-lookup"><span data-stu-id="5b501-535">trademark</span></span>             | <span data-ttu-id="5b501-536">Paramount obrázky Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="5b501-536">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="5b501-537">Ukázková aplikace volání `GetSection` s `starship` klíč.</span><span class="sxs-lookup"><span data-stu-id="5b501-537">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="5b501-538">`starship` Páry klíč hodnota jsou izolované.</span><span class="sxs-lookup"><span data-stu-id="5b501-538">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="5b501-539">`Bind` Metoda je volána v podčásti předávání v instanci `Starship` třídy.</span><span class="sxs-lookup"><span data-stu-id="5b501-539">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="5b501-540">Po navázání hodnoty instanci, instance přidruženo k vlastnosti pro vykreslování:</span><span class="sxs-lookup"><span data-stu-id="5b501-540">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="5b501-541">Vytvořit vazbu grafu objektu</span><span class="sxs-lookup"><span data-stu-id="5b501-541">Bind to an object graph</span></span>

<span data-ttu-id="5b501-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> je schopen vazby celého grafu objektů POCO.</span><span class="sxs-lookup"><span data-stu-id="5b501-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="5b501-543">Obsahuje ukázku `TvShow` modelu, jehož graf objektu obsahuje `Metadata` a `Actors` třídy (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="5b501-543">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5b501-544">Ukázková aplikace má *tvshow.xml* soubor, který obsahuje konfigurační data:</span><span class="sxs-lookup"><span data-stu-id="5b501-544">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="5b501-545">Konfigurace je vázán na celý `TvShow` grafu objektu s `Bind` metody.</span><span class="sxs-lookup"><span data-stu-id="5b501-545">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="5b501-546">Vázané instance je přiřazená k vlastnosti pro vykreslování:</span><span class="sxs-lookup"><span data-stu-id="5b501-546">The bound instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
viewModel.TvShow = tvShow;
```

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="5b501-547">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) váže a vrátí hodnotu zadaného typu.</span><span class="sxs-lookup"><span data-stu-id="5b501-547">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="5b501-548">`Get<T>` je výhodnější než použití `Bind`.</span><span class="sxs-lookup"><span data-stu-id="5b501-548">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="5b501-549">Následující kód ukazuje, jak používat `Get<T>` z předchozího příkladu, který umožňuje vázané instance má být přímo přiřazeni k vlastnosti pro vykreslování použit:</span><span class="sxs-lookup"><span data-stu-id="5b501-549">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="5b501-550">Svázat pole třídy</span><span class="sxs-lookup"><span data-stu-id="5b501-550">Bind an array to a class</span></span>

<span data-ttu-id="5b501-551">*Ukázková aplikace předvádí koncepty je popsáno v této části.*</span><span class="sxs-lookup"><span data-stu-id="5b501-551">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="5b501-552"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Podporuje pole vazby na objekty pomocí indexy pole v konfigurační klíče.</span><span class="sxs-lookup"><span data-stu-id="5b501-552">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="5b501-553">Jakékoli pole formátu, který poskytuje číselné segment klíče (`:0:`, `:1:`, &hellip; `:{n}:`) je schopný vazba pole na pole třídy POCO.</span><span class="sxs-lookup"><span data-stu-id="5b501-553">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="5b501-554">Vazba je poskytována konvence.</span><span class="sxs-lookup"><span data-stu-id="5b501-554">Binding is provided by convention.</span></span> <span data-ttu-id="5b501-555">Vlastní zprostředkovatelé konfigurace není nutné implementovat pole vazby.</span><span class="sxs-lookup"><span data-stu-id="5b501-555">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="5b501-556">**Zpracování v paměti pole**</span><span class="sxs-lookup"><span data-stu-id="5b501-556">**In-memory array processing**</span></span>

<span data-ttu-id="5b501-557">Vezměte v úvahu konfigurační klíče a hodnoty uvedené v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="5b501-557">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="5b501-558">Key</span><span class="sxs-lookup"><span data-stu-id="5b501-558">Key</span></span>     | <span data-ttu-id="5b501-559">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5b501-559">Value</span></span>  |
| :-----: | :----: |
| <span data-ttu-id="5b501-560">pole: 0</span><span class="sxs-lookup"><span data-stu-id="5b501-560">array:0</span></span> | <span data-ttu-id="5b501-561">gamma0</span><span class="sxs-lookup"><span data-stu-id="5b501-561">value0</span></span> |
| <span data-ttu-id="5b501-562">pole: 1</span><span class="sxs-lookup"><span data-stu-id="5b501-562">array:1</span></span> | <span data-ttu-id="5b501-563">Hodnota1</span><span class="sxs-lookup"><span data-stu-id="5b501-563">value1</span></span> |
| <span data-ttu-id="5b501-564">pole: 2</span><span class="sxs-lookup"><span data-stu-id="5b501-564">array:2</span></span> | <span data-ttu-id="5b501-565">Hodnota2</span><span class="sxs-lookup"><span data-stu-id="5b501-565">value2</span></span> |
| <span data-ttu-id="5b501-566">pole: 4</span><span class="sxs-lookup"><span data-stu-id="5b501-566">array:4</span></span> | <span data-ttu-id="5b501-567">value4</span><span class="sxs-lookup"><span data-stu-id="5b501-567">value4</span></span> |
| <span data-ttu-id="5b501-568">pole: 5</span><span class="sxs-lookup"><span data-stu-id="5b501-568">array:5</span></span> | <span data-ttu-id="5b501-569">Hodnota5</span><span class="sxs-lookup"><span data-stu-id="5b501-569">value5</span></span> |

<span data-ttu-id="5b501-570">Tyto klíče a hodnoty jsou načteny v ukázkové aplikaci pomocí zprostředkovatele konfigurace paměti:</span><span class="sxs-lookup"><span data-stu-id="5b501-570">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="5b501-571">Pole přeskočí hodnotu pro index &num;3.</span><span class="sxs-lookup"><span data-stu-id="5b501-571">The array skips a value for index &num;3.</span></span> <span data-ttu-id="5b501-572">Konfigurace vazače není schopen vazby hodnoty null a vytvářet položky s hodnotou null v vázaným objektům, zrušte v okamžiku, kdy je znázorněn výsledek vazby toto pole na objekt, který se stane.</span><span class="sxs-lookup"><span data-stu-id="5b501-572">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="5b501-573">V ukázkové aplikaci je k dispozici pro uložení vázané konfigurační data POCO třídy:</span><span class="sxs-lookup"><span data-stu-id="5b501-573">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5b501-574">Konfigurační data je vázaná na objekt:</span><span class="sxs-lookup"><span data-stu-id="5b501-574">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="5b501-575">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntaxe je také možné, povede k kompaktnějším kód:</span><span class="sxs-lookup"><span data-stu-id="5b501-575">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="5b501-576">Vázaný objekt, instance `ArrayExample`, přijímá data pole z konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5b501-576">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="5b501-577">`ArrayExamples.Entries` Index</span><span class="sxs-lookup"><span data-stu-id="5b501-577">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="5b501-578">`ArrayExamples.Entries` Hodnota</span><span class="sxs-lookup"><span data-stu-id="5b501-578">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="5b501-579">0</span><span class="sxs-lookup"><span data-stu-id="5b501-579">0</span></span>                             | <span data-ttu-id="5b501-580">gamma0</span><span class="sxs-lookup"><span data-stu-id="5b501-580">value0</span></span>                        |
| <span data-ttu-id="5b501-581">1</span><span class="sxs-lookup"><span data-stu-id="5b501-581">1</span></span>                             | <span data-ttu-id="5b501-582">Hodnota1</span><span class="sxs-lookup"><span data-stu-id="5b501-582">value1</span></span>                        |
| <span data-ttu-id="5b501-583">2</span><span class="sxs-lookup"><span data-stu-id="5b501-583">2</span></span>                             | <span data-ttu-id="5b501-584">Hodnota2</span><span class="sxs-lookup"><span data-stu-id="5b501-584">value2</span></span>                        |
| <span data-ttu-id="5b501-585">3</span><span class="sxs-lookup"><span data-stu-id="5b501-585">3</span></span>                             | <span data-ttu-id="5b501-586">value4</span><span class="sxs-lookup"><span data-stu-id="5b501-586">value4</span></span>                        |
| <span data-ttu-id="5b501-587">4</span><span class="sxs-lookup"><span data-stu-id="5b501-587">4</span></span>                             | <span data-ttu-id="5b501-588">Hodnota5</span><span class="sxs-lookup"><span data-stu-id="5b501-588">value5</span></span>                        |

<span data-ttu-id="5b501-589">Index &num;3 v vázaný objekt obsahuje konfigurační data pro `array:4` konfiguračního klíče a jeho hodnota `value4`.</span><span class="sxs-lookup"><span data-stu-id="5b501-589">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="5b501-590">Když je vytvořena vazba konfigurační data obsahující pole, indexy pole v konfigurační klíče se používají pouze k iteraci konfiguračních dat při vytváření objektu.</span><span class="sxs-lookup"><span data-stu-id="5b501-590">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="5b501-591">Hodnotu null nelze uchovávat v konfiguračních datech a položku s hodnotou null není vytvořená v vázaný objekt, když přeskočí jednu nebo více indexy pole v konfigurační klíče.</span><span class="sxs-lookup"><span data-stu-id="5b501-591">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="5b501-592">Chybějící položky konfigurace pro index &num;3 může být zadán před vazbu `ArrayExamples` instance poskytovatelem žádné konfigurace, která vytváří správné páru klíč hodnota v konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="5b501-592">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExamples` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="5b501-593">Pokud vzorek zahrnuje další poskytovatele konfigurace JSON s chybějící páru klíč hodnota, `ArrayExamples.Entries` odpovídá poli kompletní konfigurace:</span><span class="sxs-lookup"><span data-stu-id="5b501-593">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExamples.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="5b501-594">*missing_value.JSON*:</span><span class="sxs-lookup"><span data-stu-id="5b501-594">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5b501-595">V `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="5b501-595">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5b501-596">V `Startup` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="5b501-596">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="5b501-597">Dvojice klíč hodnota v tabulce se načtou do konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5b501-597">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="5b501-598">Key</span><span class="sxs-lookup"><span data-stu-id="5b501-598">Key</span></span>             | <span data-ttu-id="5b501-599">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5b501-599">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="5b501-600">pole: položek: 3</span><span class="sxs-lookup"><span data-stu-id="5b501-600">array:entries:3</span></span> | <span data-ttu-id="5b501-601">hodnota3</span><span class="sxs-lookup"><span data-stu-id="5b501-601">value3</span></span> |

<span data-ttu-id="5b501-602">Pokud `ArrayExamples` instance třídy je vázán po poskytovatel konfigurace JSON obsahuje položku pro index &num;3, `ArrayExamples.Entries` pole obsahuje hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5b501-602">If the `ArrayExamples` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExamples.Entries` array includes the value.</span></span>

| <span data-ttu-id="5b501-603">`ArrayExamples.Entries` Index</span><span class="sxs-lookup"><span data-stu-id="5b501-603">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="5b501-604">`ArrayExamples.Entries` Hodnota</span><span class="sxs-lookup"><span data-stu-id="5b501-604">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="5b501-605">0</span><span class="sxs-lookup"><span data-stu-id="5b501-605">0</span></span>                             | <span data-ttu-id="5b501-606">gamma0</span><span class="sxs-lookup"><span data-stu-id="5b501-606">value0</span></span>                        |
| <span data-ttu-id="5b501-607">1</span><span class="sxs-lookup"><span data-stu-id="5b501-607">1</span></span>                             | <span data-ttu-id="5b501-608">Hodnota1</span><span class="sxs-lookup"><span data-stu-id="5b501-608">value1</span></span>                        |
| <span data-ttu-id="5b501-609">2</span><span class="sxs-lookup"><span data-stu-id="5b501-609">2</span></span>                             | <span data-ttu-id="5b501-610">Hodnota2</span><span class="sxs-lookup"><span data-stu-id="5b501-610">value2</span></span>                        |
| <span data-ttu-id="5b501-611">3</span><span class="sxs-lookup"><span data-stu-id="5b501-611">3</span></span>                             | <span data-ttu-id="5b501-612">hodnota3</span><span class="sxs-lookup"><span data-stu-id="5b501-612">value3</span></span>                        |
| <span data-ttu-id="5b501-613">4</span><span class="sxs-lookup"><span data-stu-id="5b501-613">4</span></span>                             | <span data-ttu-id="5b501-614">value4</span><span class="sxs-lookup"><span data-stu-id="5b501-614">value4</span></span>                        |
| <span data-ttu-id="5b501-615">5</span><span class="sxs-lookup"><span data-stu-id="5b501-615">5</span></span>                             | <span data-ttu-id="5b501-616">Hodnota5</span><span class="sxs-lookup"><span data-stu-id="5b501-616">value5</span></span>                        |

<span data-ttu-id="5b501-617">**Zpracování pole JSON**</span><span class="sxs-lookup"><span data-stu-id="5b501-617">**JSON array processing**</span></span>

<span data-ttu-id="5b501-618">Pokud soubor JSON obsahuje pole, vytvoří se pro prvky pole s indexem založený na nule části konfigurační klíče.</span><span class="sxs-lookup"><span data-stu-id="5b501-618">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="5b501-619">V následující konfigurační soubor `subsection` je pole:</span><span class="sxs-lookup"><span data-stu-id="5b501-619">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="5b501-620">Zprostředkovatel konfigurace JSON čte konfigurační data do následující páry klíč hodnota:</span><span class="sxs-lookup"><span data-stu-id="5b501-620">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="5b501-621">Key</span><span class="sxs-lookup"><span data-stu-id="5b501-621">Key</span></span>                     | <span data-ttu-id="5b501-622">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5b501-622">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="5b501-623">json_array:Key</span><span class="sxs-lookup"><span data-stu-id="5b501-623">json_array:key</span></span>          | <span data-ttu-id="5b501-624">Hodnotaa</span><span class="sxs-lookup"><span data-stu-id="5b501-624">valueA</span></span> |
| <span data-ttu-id="5b501-625">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="5b501-625">json_array:subsection:0</span></span> | <span data-ttu-id="5b501-626">Hodnotab</span><span class="sxs-lookup"><span data-stu-id="5b501-626">valueB</span></span> |
| <span data-ttu-id="5b501-627">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="5b501-627">json_array:subsection:1</span></span> | <span data-ttu-id="5b501-628">valueC</span><span class="sxs-lookup"><span data-stu-id="5b501-628">valueC</span></span> |
| <span data-ttu-id="5b501-629">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="5b501-629">json_array:subsection:2</span></span> | <span data-ttu-id="5b501-630">Vážíme si toho</span><span class="sxs-lookup"><span data-stu-id="5b501-630">valueD</span></span> |

<span data-ttu-id="5b501-631">V ukázkové aplikaci je k dispozici pro vazbu páry klíč hodnota konfigurace následující třídy POCO:</span><span class="sxs-lookup"><span data-stu-id="5b501-631">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5b501-632">Po vytvoření vazby, `JsonArrayExample.Key` obsahuje hodnotu `valueA`.</span><span class="sxs-lookup"><span data-stu-id="5b501-632">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="5b501-633">Dílčí část hodnoty jsou uloženy ve vlastnosti pole POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="5b501-633">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="5b501-634">`JsonArrayExample.Subsection` Index</span><span class="sxs-lookup"><span data-stu-id="5b501-634">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="5b501-635">`JsonArrayExample.Subsection` Hodnota</span><span class="sxs-lookup"><span data-stu-id="5b501-635">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="5b501-636">0</span><span class="sxs-lookup"><span data-stu-id="5b501-636">0</span></span>                                   | <span data-ttu-id="5b501-637">Hodnotab</span><span class="sxs-lookup"><span data-stu-id="5b501-637">valueB</span></span>                              |
| <span data-ttu-id="5b501-638">1</span><span class="sxs-lookup"><span data-stu-id="5b501-638">1</span></span>                                   | <span data-ttu-id="5b501-639">valueC</span><span class="sxs-lookup"><span data-stu-id="5b501-639">valueC</span></span>                              |
| <span data-ttu-id="5b501-640">2</span><span class="sxs-lookup"><span data-stu-id="5b501-640">2</span></span>                                   | <span data-ttu-id="5b501-641">Vážíme si toho</span><span class="sxs-lookup"><span data-stu-id="5b501-641">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="5b501-642">Vlastního poskytovatele konfigurace</span><span class="sxs-lookup"><span data-stu-id="5b501-642">Custom configuration provider</span></span>

<span data-ttu-id="5b501-643">Ukázková aplikace předvádí, jak vytvořit základní konfigurace poskytovatele, který přečte dvojice klíč hodnota konfigurace z databáze pomocí [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="5b501-643">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="5b501-644">Zprostředkovatel má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="5b501-644">The provider has the following characteristics:</span></span>

* <span data-ttu-id="5b501-645">EF database v paměti se používá pro demonstrační účely.</span><span class="sxs-lookup"><span data-stu-id="5b501-645">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="5b501-646">Chcete-li použít databázi, která vyžaduje připojovací řetězec, implementovat sekundární `ConfigurationBuilder` zadat připojovací řetězec z jiného zprostředkovatele konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5b501-646">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="5b501-647">Poskytovatel přečte tabulku databáze do konfigurace při spuštění.</span><span class="sxs-lookup"><span data-stu-id="5b501-647">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="5b501-648">Zprostředkovatel nepodporuje dotazy na databázi na základě-key.</span><span class="sxs-lookup"><span data-stu-id="5b501-648">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="5b501-649">Znovu načíst na změnu není implementována, po spuštění aplikace nemá žádný vliv na konfiguraci aplikace, takže aktualizace databáze.</span><span class="sxs-lookup"><span data-stu-id="5b501-649">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="5b501-650">Definování `EFConfigurationValue` entity pro ukládání hodnoty konfigurace v databázi.</span><span class="sxs-lookup"><span data-stu-id="5b501-650">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="5b501-651">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="5b501-651">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5b501-652">Přidat `EFConfigurationContext` ukládání a přístup k nakonfigurované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5b501-652">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="5b501-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="5b501-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5b501-654">Vytvořte třídu, která implementuje <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="5b501-654">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="5b501-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="5b501-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5b501-656">Vytvoření vlastního poskytovatele konfigurace děděním z <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="5b501-656">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="5b501-657">Poskytovatel konfigurace inicializuje databáze, když je prázdný.</span><span class="sxs-lookup"><span data-stu-id="5b501-657">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="5b501-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="5b501-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5b501-659">`AddEFConfiguration` – Metoda rozšíření umožňuje zdroj konfigurace k přidání `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5b501-659">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="5b501-660">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="5b501-660">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5b501-661">Následující kód ukazuje, jak použít vlastní `EFConfigurationProvider` v *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="5b501-661">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="5b501-662">Konfigurace přístupu při spuštění</span><span class="sxs-lookup"><span data-stu-id="5b501-662">Access configuration during startup</span></span>

<span data-ttu-id="5b501-663">Vložit `IConfiguration` do `Startup` konstruktoru na hodnoty konfigurace přístupu v `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5b501-663">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5b501-664">Získat přístup ke konfiguraci v `Startup.Configure`, buď vložit `IConfiguration` přímo do metody nebo použít instanci z konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="5b501-664">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

<span data-ttu-id="5b501-665">Příklad přístup ke konfiguraci pomocí vhodné metody po spuštění najdete v tématu [spuštění aplikace: vhodné metody](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="5b501-665">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="5b501-666">Konfigurace přístupu v stránky Razor Pages nebo zobrazení MVC</span><span class="sxs-lookup"><span data-stu-id="5b501-666">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="5b501-667">Chcete-li získat přístup k nastavení konfigurace v zobrazení MVC nebo stránky Razor Pages, přidejte [using – direktiva](xref:mvc/views/razor#using) ([referenční dokumentace jazyka C#: použití direktivy](/dotnet/csharp/language-reference/keywords/using-directive)) pro [Microsoft.Extensions.Configuration obor názvů ](xref:Microsoft.Extensions.Configuration) a vložit <xref:Microsoft.Extensions.Configuration.IConfiguration> do stránky nebo zobrazení.</span><span class="sxs-lookup"><span data-stu-id="5b501-667">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="5b501-668">Na stránce pro stránky Razor:</span><span class="sxs-lookup"><span data-stu-id="5b501-668">In a Razor Pages page:</span></span>

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
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="5b501-669">V zobrazení MVC:</span><span class="sxs-lookup"><span data-stu-id="5b501-669">In an MVC view:</span></span>

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
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="5b501-670">Přidat konfiguraci z externího sestavení</span><span class="sxs-lookup"><span data-stu-id="5b501-670">Add configuration from an external assembly</span></span>

<span data-ttu-id="5b501-671"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> Implementace umožňuje přidání vylepšení do aplikace při spuštění z externího sestavení mimo aplikaci prvku `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="5b501-671">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="5b501-672">Další informace naleznete v tématu <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="5b501-672">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5b501-673">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5b501-673">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="5b501-674">Podrobné informace o konfiguraci Microsoft</span><span class="sxs-lookup"><span data-stu-id="5b501-674">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
