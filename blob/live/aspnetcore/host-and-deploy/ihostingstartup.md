---
title: "Přidání funkce aplikací z externí sestavení pomocí IHostingStartup v ASP.NET Core"
author: guardrex
description: "Postup přidání funkce do aplikace ASP.NET Core ze externí sestavení pomocí implementace IHostingStartup zjistit."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 12/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/ihostingstartup
ms.openlocfilehash: 6013091d94302f0f9ecbc777d3ef64774b49aebe
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="add-app-features-from-an-external-assembly-using-ihostingstartup-in-aspnet-core"></a><span data-ttu-id="b9c50-103">Přidání funkce aplikací z externí sestavení pomocí IHostingStartup v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9c50-103">Add app features from an external assembly using IHostingStartup in ASP.NET Core</span></span>

<span data-ttu-id="b9c50-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b9c50-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b9c50-105">[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementace umožňuje přidávání funkcí do aplikace při spuštění z mimo aplikace `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="b9c50-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding features to an app at startup from outside of the app's `Startup` class.</span></span> <span data-ttu-id="b9c50-106">Například můžete použít knihovny externích nástrojů `IHostingStartup` implementace je poskytnout další konfigurace zprostředkovatele nebo služby do aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9c50-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="b9c50-107">`IHostingStartup`*je k dispozici v technologii ASP.NET Core 2.0 nebo novější.*</span><span class="sxs-lookup"><span data-stu-id="b9c50-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="b9c50-108">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b9c50-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="b9c50-109">Zjistit načíst hostování spuštění sestavení</span><span class="sxs-lookup"><span data-stu-id="b9c50-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="b9c50-110">Pokud chcete zjistit, že hostování spuštění sestavení načíst aplikaci nebo knihovny, povolte protokolování a zkontrolujte protokoly aplikací.</span><span class="sxs-lookup"><span data-stu-id="b9c50-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="b9c50-111">Jsou protokolovány chyb vzniklých při načítání sestavení.</span><span class="sxs-lookup"><span data-stu-id="b9c50-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="b9c50-112">Načíst hostování spuštění sestavení se protokolují na úrovni ladění a jsou zaznamenány všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="b9c50-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="b9c50-113">Čtení ukázkové aplikace [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) do `string` pole a v indexu stránku aplikace zobrazí výsledky:</span><span class="sxs-lookup"><span data-stu-id="b9c50-113">The sample app reads the the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[Main](ihostingstartup/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="b9c50-114">Zakázat automatické načítání hostování spuštění sestavení</span><span class="sxs-lookup"><span data-stu-id="b9c50-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="b9c50-115">Existují dva způsoby, jak zakázat automatické načítání hostování spuštění sestavení:</span><span class="sxs-lookup"><span data-stu-id="b9c50-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="b9c50-116">Nastavte [zabránit spuštění hostování](xref:fundamentals/hosting#prevent-hosting-startup) hostitele nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b9c50-116">Set the [Prevent Hosting Startup](xref:fundamentals/hosting#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="b9c50-117">Nastavte `ASPNETCORE_preventHostingStartup` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="b9c50-117">Set the `ASPNETCORE_preventHostingStartup` environment variable.</span></span>

<span data-ttu-id="b9c50-118">Když nastavení hostitele nebo proměnná prostředí je nastavená na `true` nebo `1`, hostování spuštění sestavení nejsou automaticky načíst.</span><span class="sxs-lookup"><span data-stu-id="b9c50-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="b9c50-119">Pokud jsou obě nastaveny, nastavení hostitele řídí chování.</span><span class="sxs-lookup"><span data-stu-id="b9c50-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="b9c50-120">Zakázání hostování spuštění sestavení pomocí proměnné prostředí nebo nastavení hostitele je globálně zakáže a může zakázat několik funkcí aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9c50-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several features of an app.</span></span> <span data-ttu-id="b9c50-121">Momentálně nelze k selektivnímu zakázání hostování spuštění sestavení přidal do knihovny, pokud knihovny nabízí možnost vlastní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b9c50-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="b9c50-122">Budoucí verze bude umožňují selektivně zakázat hostování spuštění sestavení (viz [Githubu vydání aspnet nebo hostitelský #1243](https://github.com/aspnet/Hosting/pull/1243)).</span><span class="sxs-lookup"><span data-stu-id="b9c50-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup-features"></a><span data-ttu-id="b9c50-123">Implementovat IHostingStartup funkce</span><span class="sxs-lookup"><span data-stu-id="b9c50-123">Implement IHostingStartup features</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="b9c50-124">Vytvořit sestavení</span><span class="sxs-lookup"><span data-stu-id="b9c50-124">Create the assembly</span></span>

<span data-ttu-id="b9c50-125">`IHostingStartup` Nasazení funkce jako sestavení podle konzolovou aplikaci bez vstupního bodu.</span><span class="sxs-lookup"><span data-stu-id="b9c50-125">An `IHostingStartup` feature is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="b9c50-126">Odkazy na sestavení [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) balíčku:</span><span class="sxs-lookup"><span data-stu-id="b9c50-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[Main](ihostingstartup/snapshot_sample/StartupFeature.csproj)]

<span data-ttu-id="b9c50-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atribut určuje třídu jako implementaci `IHostingStartup` pro načítání a spouštění při sestavování [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="b9c50-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="b9c50-128">V následujícím příkladu je obor názvů `StartupFeature`, a třída je `StartupFeatureHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="b9c50-128">In the following example, the namespace is `StartupFeature`, and the class is `StartupFeatureHostingStartup`:</span></span>

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet1)]

<span data-ttu-id="b9c50-129">Implementuje třídu `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="b9c50-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="b9c50-130">Třída [konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metoda používá [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) pro přidání funkcí do aplikace:</span><span class="sxs-lookup"><span data-stu-id="b9c50-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add features to an app:</span></span>

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="b9c50-131">Při vytváření `IHostingStartup` souboru závislosti projektu (*\\*. deps.json*) nastaví `runtime` umístění sestavení do *bin* složky:</span><span class="sxs-lookup"><span data-stu-id="b9c50-131">When building an `IHostingStartup` project, the dependencies file (*\\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="b9c50-132">Zobrazí se jenom část souboru.</span><span class="sxs-lookup"><span data-stu-id="b9c50-132">Only part of the file is shown.</span></span> <span data-ttu-id="b9c50-133">Název sestavení v příkladu je `StartupFeature`.</span><span class="sxs-lookup"><span data-stu-id="b9c50-133">The assembly name in the example is `StartupFeature`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="b9c50-134">Aktualizovat soubor závislosti</span><span class="sxs-lookup"><span data-stu-id="b9c50-134">Update the dependencies file</span></span>

<span data-ttu-id="b9c50-135">Modul runtime umístění je určeno v  *\\*. deps.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="b9c50-135">The runtime location is specified in the *\\*.deps.json* file.</span></span> <span data-ttu-id="b9c50-136">Na aktivní funkci `runtime` element musíte zadat umístění funkce runtime sestavení.</span><span class="sxs-lookup"><span data-stu-id="b9c50-136">To active the feature, the `runtime` element must specify the location of the feature's runtime assembly.</span></span> <span data-ttu-id="b9c50-137">Předpony `runtime` umístění s `lib/netcoreapp2.0/`:</span><span class="sxs-lookup"><span data-stu-id="b9c50-137">Prefix the `runtime` location with `lib/netcoreapp2.0/`:</span></span>

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="b9c50-138">V ukázkové aplikace, změna  *\\*. deps.json* souboru provádí [prostředí PowerShell](/powershell/scripting/powershell-scripting) skriptu.</span><span class="sxs-lookup"><span data-stu-id="b9c50-138">In the sample app, modification of the *\\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="b9c50-139">Skript prostředí PowerShell automaticky spuštěn sestavení cíl v souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="b9c50-139">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="feature-activation"></a><span data-ttu-id="b9c50-140">Aktivace funkce</span><span class="sxs-lookup"><span data-stu-id="b9c50-140">Feature activation</span></span>

<span data-ttu-id="b9c50-141">**Umístěte soubor sestavení**</span><span class="sxs-lookup"><span data-stu-id="b9c50-141">**Place the assembly file**</span></span>

<span data-ttu-id="b9c50-142">`IHostingStartup` Musí být soubor sestavení je implementace *bin*-nasazené v aplikaci nebo umístěny do [runtime úložiště](/dotnet/core/deploying/runtime-store):</span><span class="sxs-lookup"><span data-stu-id="b9c50-142">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="b9c50-143">Pro použití na uživatele umístíte v úložišti runtime profil uživatele v sestavení:</span><span class="sxs-lookup"><span data-stu-id="b9c50-143">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="b9c50-144">Pro globální použití umístíte sestavení v instalaci .NET Core runtime úložiště:</span><span class="sxs-lookup"><span data-stu-id="b9c50-144">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="b9c50-145">Při nasazování sestavení do úložiště modulu runtime, soubor symboly může také nasadit, ale není nutné u funkce fungovat.</span><span class="sxs-lookup"><span data-stu-id="b9c50-145">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the feature to work.</span></span>

<span data-ttu-id="b9c50-146">**Umístěte soubor závislosti**</span><span class="sxs-lookup"><span data-stu-id="b9c50-146">**Place the dependencies file**</span></span>

<span data-ttu-id="b9c50-147">Implementace rozhraní  *\\*. deps.json* soubor musí být na dostupném místě.</span><span class="sxs-lookup"><span data-stu-id="b9c50-147">The implementation's *\\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="b9c50-148">Pro použití na uživatele, umístěte soubor v `additonalDeps` složku profilu uživatele `.dotnet` nastavení:</span><span class="sxs-lookup"><span data-stu-id="b9c50-148">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span> 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="b9c50-149">Pro globální použití, umístěte soubor v `additonalDeps` do složky instalace .NET Core:</span><span class="sxs-lookup"><span data-stu-id="b9c50-149">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="b9c50-150">Všimněte si, jeho verzi, `2.0.0`, odpovídat verzi aplikace sdílený modul runtime, který používá cílové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9c50-150">Note the version, `2.0.0`, reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="b9c50-151">Sdílený modul runtime se zobrazí v  *\\*. runtimeconfig.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="b9c50-151">The shared runtime is shown in the *\\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="b9c50-152">V ukázkové aplikace je sdílený modul runtime specifikován v *HostingStartupSample.runtimeconfig.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="b9c50-152">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="b9c50-153">**Proměnné prostředí sady**</span><span class="sxs-lookup"><span data-stu-id="b9c50-153">**Set environment variables**</span></span>

<span data-ttu-id="b9c50-154">Nastavte následující proměnné prostředí v rámci aplikaci, která používá funkci.</span><span class="sxs-lookup"><span data-stu-id="b9c50-154">Set the following environment variables in the context of the app that uses the feature.</span></span>

<span data-ttu-id="b9c50-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="b9c50-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="b9c50-156">Hledat pouze hostování spuštění sestavení `HostingStartupAttribute`.</span><span class="sxs-lookup"><span data-stu-id="b9c50-156">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="b9c50-157">Název sestavení implementace je poskytován v této proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="b9c50-157">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="b9c50-158">Ukázková aplikace nastavuje tuto hodnotu `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="b9c50-158">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="b9c50-159">Hodnotu můžete nastavit také pomocí [hostování spuštění sestavení](xref:fundamentals/hosting#hosting-startup-assemblies) hostitele nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b9c50-159">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/hosting#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="b9c50-160">DOTNET\_DALŠÍ\_DEPS</span><span class="sxs-lookup"><span data-stu-id="b9c50-160">DOTNET\_ADDITIONAL\_DEPS</span></span>

<span data-ttu-id="b9c50-161">Umístění implementace  *\\*. deps.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="b9c50-161">The location of the implementation's *\\*.deps.json* file.</span></span>

<span data-ttu-id="b9c50-162">Pokud soubor je umístěn v profilu uživatele *.dotnet* složku pro použití na uživatele:</span><span class="sxs-lookup"><span data-stu-id="b9c50-162">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="b9c50-163">Pokud soubor je umístěn v instalaci .NET Core pro globální použití, zadejte úplnou cestu k souboru:</span><span class="sxs-lookup"><span data-stu-id="b9c50-163">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="b9c50-164">Ukázková aplikace nastavuje tuto hodnotu:</span><span class="sxs-lookup"><span data-stu-id="b9c50-164">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="b9c50-165">Příklady způsobu nastavení proměnných prostředí pro různé operační systémy najdete v tématu [práce s několika prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="b9c50-165">For examples of how to set environment variables for various operating systems, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="b9c50-166">Ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="b9c50-166">Sample app</span></span>

<span data-ttu-id="b9c50-167">[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample)) používá `IHostingStartup` vytvořit nástroj diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="b9c50-167">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="b9c50-168">Nástroj přidá dva middlewares aplikace při spuštění, které poskytují diagnostické informace:</span><span class="sxs-lookup"><span data-stu-id="b9c50-168">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="b9c50-169">Registrované služby</span><span class="sxs-lookup"><span data-stu-id="b9c50-169">Registered services</span></span>
* <span data-ttu-id="b9c50-170">Adresa: schéma, hostitele, základ cesty, cesta, řetězec dotazu</span><span class="sxs-lookup"><span data-stu-id="b9c50-170">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="b9c50-171">Připojení: vzdálené IP, vzdálený port, místní IP adresy, místní port, klientský certifikát</span><span class="sxs-lookup"><span data-stu-id="b9c50-171">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="b9c50-172">Hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="b9c50-172">Request headers</span></span>
* <span data-ttu-id="b9c50-173">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="b9c50-173">Environment variables</span></span>

<span data-ttu-id="b9c50-174">Spustit ukázku:</span><span class="sxs-lookup"><span data-stu-id="b9c50-174">To run the sample:</span></span>

1. <span data-ttu-id="b9c50-175">Spuštění diagnostiky projektu používá [prostředí PowerShell](/powershell/scripting/powershell-scripting) změnit jeho *StartupDiagnostics.deps.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="b9c50-175">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="b9c50-176">Prostředí PowerShell je nainstalována ve výchozím nastavení u operačního systému Windows od verze Windows 7 SP1 a Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="b9c50-176">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="b9c50-177">Chcete-li získat prostředí PowerShell na jiných platformách, přečtěte si téma [instalace prostředí Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="b9c50-177">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="b9c50-178">Sestavení projektu spuštění diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="b9c50-178">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="b9c50-179">Cíl sestavení v souboru projektu:</span><span class="sxs-lookup"><span data-stu-id="b9c50-179">A build target in the project file:</span></span>
   * <span data-ttu-id="b9c50-180">Přesune sestavení a soubory do úložiště runtime profilu uživatele se symboly.</span><span class="sxs-lookup"><span data-stu-id="b9c50-180">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="b9c50-181">Spustí skript prostředí PowerShell k úpravě *StartupDiagnostics.deps.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="b9c50-181">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="b9c50-182">Přesune *StartupDiagnostics.deps.json* souboru profilu uživatele `additionalDeps` složky.</span><span class="sxs-lookup"><span data-stu-id="b9c50-182">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="b9c50-183">Nastavení proměnných prostředí:</span><span class="sxs-lookup"><span data-stu-id="b9c50-183">Set the environment variables:</span></span>
    * <span data-ttu-id="b9c50-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="b9c50-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="b9c50-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="b9c50-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="b9c50-186">Spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9c50-186">Run the sample app.</span></span>
5. <span data-ttu-id="b9c50-187">Požadavku `/services` zaregistrovat koncový bod zobrazíte aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="b9c50-187">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="b9c50-188">Požadavku `/diag` koncový bod diagnostické informace.</span><span class="sxs-lookup"><span data-stu-id="b9c50-188">Request the `/diag` endpoint to see the diagnostic information.</span></span>
