---
title: "Přidání funkce aplikací z externí sestavení pomocí IHostingStartup v ASP.NET Core"
author: guardrex
description: "Postup přidání funkce do aplikace ASP.NET Core ze externí sestavení pomocí implementace IHostingStartup zjistit."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/ihostingstartup
ms.openlocfilehash: bd2446d6133e0c06dc14509271c2d17be4c95b63
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="add-app-features-from-an-external-assembly-using-ihostingstartup-in-aspnet-core"></a>Přidání funkce aplikací z externí sestavení pomocí IHostingStartup v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementace umožňuje přidávání funkcí do aplikace při spuštění z mimo aplikace `Startup` třídy. Například můžete použít knihovny externích nástrojů `IHostingStartup` implementace je poskytnout další konfigurace zprostředkovatele nebo služby do aplikace. `IHostingStartup`*je k dispozici v technologii ASP.NET Core 2.0 nebo novější.*

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>Zjistit načíst hostování spuštění sestavení

Pokud chcete zjistit, že hostování spuštění sestavení načíst aplikaci nebo knihovny, povolte protokolování a zkontrolujte protokoly aplikací. Jsou protokolovány chyb vzniklých při načítání sestavení. Načíst hostování spuštění sestavení se protokolují na úrovni ladění a jsou zaznamenány všechny chyby.

Čtení ukázkové aplikace [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) do `string` pole a v indexu stránku aplikace zobrazí výsledky:

[!code-csharp[Main](ihostingstartup/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Zakázat automatické načítání hostování spuštění sestavení

Existují dva způsoby, jak zakázat automatické načítání hostování spuštění sestavení:

* Nastavte [zabránit spuštění hostování](xref:fundamentals/hosting#prevent-hosting-startup) hostitele nastavení konfigurace.
* Nastavte `ASPNETCORE_preventHostingStartup` proměnné prostředí.

Když nastavení hostitele nebo proměnná prostředí je nastavená na `true` nebo `1`, hostování spuštění sestavení nejsou automaticky načíst. Pokud jsou obě nastaveny, nastavení hostitele řídí chování.

Zakázání hostování spuštění sestavení pomocí proměnné prostředí nebo nastavení hostitele je globálně zakáže a může zakázat několik funkcí aplikace. Momentálně nelze k selektivnímu zakázání hostování spuštění sestavení přidal do knihovny, pokud knihovny nabízí možnost vlastní konfigurace. Budoucí verze bude umožňují selektivně zakázat hostování spuštění sestavení (viz [Githubu vydání aspnet nebo hostitelský #1243](https://github.com/aspnet/Hosting/pull/1243)).

## <a name="implement-ihostingstartup-features"></a>Implementovat IHostingStartup funkce

### <a name="create-the-assembly"></a>Vytvořit sestavení

`IHostingStartup` Nasazení funkce jako sestavení podle konzolovou aplikaci bez vstupního bodu. Odkazy na sestavení [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) balíčku:

[!code-xml[Main](ihostingstartup/snapshot_sample/StartupFeature.csproj)]

A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atribut určuje třídu jako implementaci `IHostingStartup` pro načítání a spouštění při sestavování [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). V následujícím příkladu je obor názvů `StartupFeature`, a třída je `StartupFeatureHostingStartup`:

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet1)]

Implementuje třídu `IHostingStartup`. Třída [konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metoda používá [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) pro přidání funkcí do aplikace:

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

Při vytváření `IHostingStartup` souboru závislosti projektu (*\*. deps.json*) nastaví `runtime` umístění sestavení do *bin* složky:

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

Zobrazí se jenom část souboru. Název sestavení v příkladu je `StartupFeature`.

### <a name="update-the-dependencies-file"></a>Aktualizovat soubor závislosti

Modul runtime umístění je určeno v  *\*. deps.json* souboru. Na aktivní funkci `runtime` element musíte zadat umístění funkce runtime sestavení. Předpony `runtime` umístění s `lib/netcoreapp2.0/`:

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

V ukázkové aplikace, změna  *\*. deps.json* souboru provádí [prostředí PowerShell](/powershell/scripting/powershell-scripting) skriptu. Skript prostředí PowerShell automaticky spuštěn sestavení cíl v souboru projektu.

### <a name="feature-activation"></a>Aktivace funkce

**Umístěte soubor sestavení**

`IHostingStartup` Musí být soubor sestavení je implementace *bin*-nasazené v aplikaci nebo umístěny do [runtime úložiště](/dotnet/core/deploying/runtime-store):

Pro použití na uživatele umístíte v úložišti runtime profil uživatele v sestavení:

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

Pro globální použití umístíte sestavení v instalaci .NET Core runtime úložiště:

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

Při nasazování sestavení do úložiště modulu runtime, soubor symboly může také nasadit, ale není nutné u funkce fungovat.

**Umístěte soubor závislosti**

Implementace rozhraní  *\*. deps.json* soubor musí být na dostupném místě.

Pro použití na uživatele, umístěte soubor v `additonalDeps` složku profilu uživatele `.dotnet` nastavení: 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Pro globální použití, umístěte soubor v `additonalDeps` do složky instalace .NET Core:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Všimněte si, jeho verzi, `2.0.0`, odpovídat verzi aplikace sdílený modul runtime, který používá cílové aplikace. Sdílený modul runtime se zobrazí v  *\*. runtimeconfig.json* souboru. V ukázkové aplikace je sdílený modul runtime specifikován v *HostingStartupSample.runtimeconfig.json* souboru.

**Proměnné prostředí sady**

Nastavte následující proměnné prostředí v rámci aplikaci, která používá funkci.

ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES

Hledat pouze hostování spuštění sestavení `HostingStartupAttribute`. Název sestavení implementace je poskytován v této proměnné prostředí. Ukázková aplikace nastavuje tuto hodnotu `StartupDiagnostics`.

Hodnotu můžete nastavit také pomocí [hostování spuštění sestavení](xref:fundamentals/hosting#hosting-startup-assemblies) hostitele nastavení konfigurace.

DOTNET\_DALŠÍ\_DEPS

Umístění implementace  *\*. deps.json* souboru.

Pokud soubor je umístěn v profilu uživatele *.dotnet* složku pro použití na uživatele:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

Pokud soubor je umístěn v instalaci .NET Core pro globální použití, zadejte úplnou cestu k souboru:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

Ukázková aplikace nastavuje tuto hodnotu:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

Příklady způsobu nastavení proměnných prostředí pro různé operační systémy najdete v tématu [práce s několika prostředí](xref:fundamentals/environments).

## <a name="sample-app"></a>Ukázkové aplikace

[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample)) používá `IHostingStartup` vytvořit nástroj diagnostiky. Nástroj přidá dva middlewares aplikace při spuštění, které poskytují diagnostické informace:

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
