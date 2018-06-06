---
title: Vylepšení aplikace z externí sestavení v ASP.NET Core s IHostingStartup
author: guardrex
description: Zjistit, jak rozšířit aplikace ASP.NET Core z externí sestavení pomocí IHostingStartup implementace.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: d913d8a773d312fc4c3191926c6eae2fcb7c6a3e
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734387"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>Vylepšení aplikace z externí sestavení v ASP.NET Core s IHostingStartup

Podle [Luke Latham](https://github.com/guardrex)

[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementace umožňuje přidání vylepšení do aplikace při spuštění z externí sestavení mimo aplikace `Startup` třídy. Například můžete použít knihovny externích nástrojů `IHostingStartup` implementace je poskytnout další konfigurace zprostředkovatele nebo služby do aplikace. `IHostingStartup` *je k dispozici v technologii ASP.NET Core 2.0 nebo novější.*

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>Zjistit načíst hostování spuštění sestavení

Pokud chcete zjistit, že hostování spuštění sestavení načíst aplikaci nebo knihovny, povolte protokolování a zkontrolujte protokoly aplikací. Jsou protokolovány chyb vzniklých při načítání sestavení. Načíst hostování spuštění sestavení se protokolují na úrovni ladění a jsou zaznamenány všechny chyby.

Čtení ukázkové aplikace [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) do `string` pole a v indexu stránku aplikace zobrazí výsledky:

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Zakázat automatické načítání hostování spuštění sestavení

Existují dva způsoby, jak zakázat automatické načítání hostování spuštění sestavení:

* Nastavte [zabránit spuštění hostování](xref:fundamentals/host/web-host#prevent-hosting-startup) hostitele nastavení konfigurace.
* Nastavte `ASPNETCORE_PREVENTHOSTINGSTARTUP` proměnné prostředí.

Když nastavení hostitele nebo proměnná prostředí je nastavená na `true` nebo `1`, hostování spuštění sestavení nejsou automaticky načíst. Pokud jsou obě nastaveny, nastavení hostitele řídí chování.

Zakázání hostování spuštění sestavení pomocí proměnné prostředí nebo nastavení hostitele je globálně zakáže a může zakázat několik vlastností aplikace. Momentálně nelze k selektivnímu zakázání hostování spuštění sestavení přidal do knihovny, pokud knihovny nabízí možnost vlastní konfigurace. Budoucí verze bude umožňují selektivně zakázat hostování spuštění sestavení (viz [Githubu vydání aspnet nebo hostitelský #1243](https://github.com/aspnet/Hosting/pull/1243)).

## <a name="implement-ihostingstartup"></a>Implementace IHostingStartup

### <a name="create-the-assembly"></a>Vytvořit sestavení

`IHostingStartup` Vylepšení nasazení jako sestavení podle konzolovou aplikaci bez vstupního bodu. Odkazy na sestavení [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) balíčku:

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atribut určuje třídu jako implementaci `IHostingStartup` pro načítání a spouštění při sestavování [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). V následujícím příkladu je obor názvů `StartupEnhancement`, a třída je `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

Implementuje třídu `IHostingStartup`. Třída [konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metoda používá [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) přidání vylepšení do aplikace:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

Při vytváření `IHostingStartup` souboru závislosti projektu (*\*. deps.json*) nastaví `runtime` umístění sestavení do *bin* složky:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Zobrazí se jenom část souboru. Název sestavení v příkladu je `StartupEnhancement`.

### <a name="update-the-dependencies-file"></a>Aktualizovat soubor závislosti

Modul runtime umístění je určeno v  *\*. deps.json* souboru. Na aktivní vylepšení `runtime` element musíte zadat umístění vylepšení modulu runtime sestavení. Předpony `runtime` umístění s `lib/<TARGET_FRAMEWORK_MONIKER>/`:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

V ukázkové aplikace, změna  *\*. deps.json* souboru provádí [prostředí PowerShell](/powershell/scripting/powershell-scripting) skriptu. Skript prostředí PowerShell automaticky spuštěn sestavení cíl v souboru projektu.

### <a name="enhancement-activation"></a>Vylepšení aktivace

**Umístěte soubor sestavení**

`IHostingStartup` Musí být soubor sestavení je implementace *bin*-nasazené v aplikaci nebo umístěny do [runtime úložiště](/dotnet/core/deploying/runtime-store):

Pro použití na uživatele umístíte v úložišti runtime profil uživatele v sestavení:

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

Pro globální použití umístíte sestavení v instalaci .NET Core runtime úložiště:

```
<DRIVE>\Program Files\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

Při nasazování sestavení do úložiště modulu runtime, soubor symboly může také nasadit, ale není nutné u vylepšení pracovat.

**Umístěte soubor závislosti**

Implementace rozhraní  *\*. deps.json* soubor musí být na dostupném místě.

Pro použití na uživatele, umístěte soubor v `additonalDeps` složku profilu uživatele `.dotnet` nastavení:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

Pro globální použití, umístěte soubor v `additonalDeps` do složky instalace .NET Core:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

Verze sdílený framework odráží verzi sdílený modul runtime, který používá cílové aplikace. Sdílený modul runtime se zobrazí v  *\*. runtimeconfig.json* souboru. V ukázkové aplikace je sdílený modul runtime specifikován v *HostingStartupSample.runtimeconfig.json* souboru.

**Proměnné prostředí sady**

Nastavte následující proměnné prostředí v rámci aplikaci, která používá vylepšení.

ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES

Hledat pouze hostování spuštění sestavení `HostingStartupAttribute`. Název sestavení implementace je poskytován v této proměnné prostředí. Ukázková aplikace nastavuje tuto hodnotu `StartupDiagnostics`.

Hodnotu můžete nastavit také pomocí [hostování spuštění sestavení](xref:fundamentals/host/web-host#hosting-startup-assemblies) hostitele nastavení konfigurace.

DOTNET\_DALŠÍ\_DEPS

Umístění implementace  *\*. deps.json* souboru.

Pokud soubor je umístěn v profilu uživatele *.dotnet* složku pro použití na uživatele:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

Pokud soubor je umístěn v instalaci .NET Core pro globální použití, zadejte úplnou cestu k souboru:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

Ukázková aplikace nastavuje tuto hodnotu:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

Příklady způsobu nastavení proměnných prostředí pro různé operační systémy najdete v tématu [použijte prostředí s více](xref:fundamentals/environments).

## <a name="sample-app"></a>Ukázkové aplikace

[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample)) používá `IHostingStartup` vytvořit nástroj diagnostiky. Nástroj přidá dva middlewares aplikace při spuštění, které poskytují diagnostické informace:

* Registrované služby
* Adresa: schéma, hostitele, základ cesty, cesta, řetězec dotazu
* Připojení: vzdálené IP, vzdálený port, místní IP adresy, místní port, klientský certifikát
* Hlavičky požadavku
* Proměnné prostředí

Spustit ukázku:

1. Spuštění diagnostiky projektu používá [prostředí PowerShell](/powershell/scripting/powershell-scripting) změnit jeho *StartupDiagnostics.deps.json* souboru. Prostředí PowerShell je nainstalována ve výchozím nastavení u operačního systému Windows od verze Windows 7 SP1 a Windows Server 2008 R2 SP1. Chcete-li získat prostředí PowerShell na jiných platformách, přečtěte si téma [instalace prostředí Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).
2. Sestavení projektu spuštění diagnostiky. Cíl sestavení v souboru projektu:
   * Přesune sestavení a soubory do úložiště runtime profilu uživatele se symboly.
   * Spustí skript prostředí PowerShell k úpravě *StartupDiagnostics.deps.json* souboru.
   * Přesune *StartupDiagnostics.deps.json* souboru profilu uživatele `additionalDeps` složky.
3. Nastavení proměnných prostředí:
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. Spuštění ukázkové aplikace.
5. Požadavku `/services` zaregistrovat koncový bod zobrazíte aplikace služby. Požadavku `/diag` koncový bod diagnostické informace.
