---
title: Vylepšení aplikace z externího sestavení v ASP.NET Core s IHostingStartup
author: guardrex
description: Zjistěte, jak rozšířit aplikace ASP.NET Core z externího sestavení přes IHostingStartup implementace.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: a06c2da04c1631f5811a535c891ca5190b0d8864
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207534"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>Vylepšení aplikace z externího sestavení v ASP.NET Core s IHostingStartup

Podle [Luke Latham](https://github.com/guardrex)

[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (který je hostitelem spouštěcí) implementace vylepšení přidá do aplikace při spuštění z externího sestavení. Například externí knihovnu můžete hostování implementace spuštění uvést další konfigurace zprostředkovatele nebo služby do aplikace. `IHostingStartup` *je k dispozici v ASP.NET Core 2.0 nebo novější.*

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([stažení](xref:index#how-to-download-a-sample))

## <a name="hostingstartup-attribute"></a>Atribut HostingStartup

A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atribut indikuje přítomnost hostování sestavení po spuštění k aktivaci za běhu.

Vstupní sestavení nebo sestavení obsahující `Startup` třídy automaticky vyhledávat `HostingStartup` atribut. Seznam sestavení, které chcete hledat `HostingStartup` atributy je načtená v době běhu z konfigurace v [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey). Načíst seznam sestavení, které chcete vyloučit ze zjišťování z [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey). Další informace najdete v tématu [webového hostitele: hostování při spuštění sestavení](xref:fundamentals/host/web-host#hosting-startup-assemblies) a [webového hostitele: sestavení vyloučit při spuštění hostování](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).

V následujícím příkladu, obor názvů hostování sestavení po spuštění je `StartupEnhancement`. Třída obsahující kód hostování spuštění je `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

`HostingStartup` Atribut se obvykle nachází v hostitelském sestavení po spuštění `IHostingStartup` souboru implementace třídy.

## <a name="discover-loaded-hosting-startup-assemblies"></a>Zjistit načtená hostingu při spuštění sestavení

Ke zřízení načtená hostingu při spuštění sestavení, povolte protokolování a v protokolech aplikace. Jsou zaznamenány chyby, ke kterým dochází při načítání sestavení. Načtená hostingu při spuštění sestavení jsou zaznamenány na úrovni ladění a jsou zaznamenány všechny chyby.

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Vypnout automatické načítání hostování při spuštění sestavení

::: moniker range=">= aspnetcore-2.1"

Vypnout automatické načítání hostování při spuštění sestavení, použijte jednu z následujících postupů:

* Abyste zabránili všechna sestavení po spuštění hostování načítání, nastavte jednu z následujících způsobů `true` nebo `1`:
  * [Zabránit spuštění hostování](xref:fundamentals/host/web-host#prevent-hosting-startup) hostovat nastavení konfigurace.
  * `ASPNETCORE_PREVENTHOSTINGSTARTUP` proměnné prostředí.
* Pokud chcete zabránit konkrétní hostingu při spuštění sestavení z načítání, nastavte na řetězec oddělený středníkem hostování při spuštění sestavení mají vyloučit při spuštění jednu z následujících:
  * [Hostování sestavení vyloučit při spuštění](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) hostovat nastavení konfigurace.
  * `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` proměnné prostředí.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Vypnout automatické načítání hostování při spuštění sestavení, nastavte jednu z následujících způsobů `true` nebo `1`:

* [Zabránit spuštění hostování](xref:fundamentals/host/web-host#prevent-hosting-startup) hostovat nastavení konfigurace.
* `ASPNETCORE_PREVENTHOSTINGSTARTUP` proměnné prostředí.

::: moniker-end

Pokud nastavení konfigurace hostitele a proměnné prostředí jsou nastaveny, nastavení hostitele řídí chování.

Zakázání hostingu při spuštění sestavení pomocí proměnná hostitele nastavení nebo prostředí globálně zakáže sestavení a může zakázat několik vlastností aplikace.

## <a name="project"></a>Projekt

Vytvoření hostitelského spouštěcího s jedním z následujících typů projektu:

* [Knihovna tříd](#class-library)
* [Konzolová aplikace bez vstupního bodu](#console-app-without-an-entry-point)

### <a name="class-library"></a>Knihovna tříd

Hostování rozšíření spuštění lze zadat v knihovně tříd. Knihovna obsahuje `HostingStartup` atribut.

[Ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) zahrnuje aplikace stránky Razor, *HostingStartupApp*a knihovny tříd, *HostingStartupLibrary*. Knihovna tříd:

* Obsahuje třídu spuštění hostování `ServiceKeyInjection`, která implementuje `IHostingStartup`. `ServiceKeyInjection` Přidá dvojici řetězce služby pro konfiguraci aplikace pomocí zprostředkovatele konfigurace v paměti ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).
* Zahrnuje `HostingStartup` atribut, který identifikuje obor názvů a třídy spuštění hostování.

`ServiceKeyInjection` Třídy [konfigurovat](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metoda používá [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) přidání vylepšení do aplikace. `IHostingStartup.Configure` v hostitelských spuštění sestavení je volána modulem runtime před `Startup.Configure` v uživatelském kódu, který umožňuje přepsat žádnou konfiguraci poskytované hostujícím sestavení při spuštění uživatelského kódu.

*HostingStartupLibrary/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

Aplikace indexovou stránku přečte a vykreslí konfigurační hodnoty pro dva klíče, nastavit podle knihovny tříd hostingu při spuštění sestavení:

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

[Ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) také zahrnuje projektu balíček NuGet, který poskytuje samostatné hostitelské spuštění *HostingStartupPackage*. Balíček má stejné charakteristiky knihovna tříd je popsáno výše. Balíček:

* Obsahuje třídu spuštění hostování `ServiceKeyInjection`, která implementuje `IHostingStartup`. `ServiceKeyInjection` Přidá dvojici řetězce služby pro konfiguraci aplikace.
* Zahrnuje `HostingStartup` atribut.

*HostingStartupPackage/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

Aplikace indexovou stránku přečte a vykreslí konfigurační hodnoty pro dva klíče nastavil hostování sestavení po spuštění balíčku:

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a>Konzolová aplikace bez vstupního bodu

*Tento přístup je dostupná jenom pro aplikace .NET Core, není rozhraní .NET Framework.*

Dynamické vylepšení spuštění hostování, který nevyžaduje odkaz kompilace pro aktivaci lze zadat v konzolové aplikaci bez vstupního bodu. Aplikace obsahuje `HostingStartup` atribut. Chcete-li vytvořit dynamické hostování spuštění:

1. K implementaci knihovně je vytvořený z třídy, která obsahuje `IHostingStartup` implementace. Implementace knihovny je považován za normálního balíčku.
1. Konzolová aplikace bez vstupního bodu odkazuje balíček knihovny implementace. Protože se používá konzolovou aplikaci:
   * Souboru závislostí je k assetu spustitelné aplikace, takže knihovnu nemůžete poskytnout soubor závislosti.
   * Knihovny nelze přímo přidat [úložiště balíčků modulu runtime](/dotnet/core/deploying/runtime-store), což vyžaduje spustitelný projekt, který cílí na sdílený modul runtime.
1. Publikování aplikace konzoly získat závislosti spuštění hostování. V důsledku publikování aplikace konzoly je, že se ze souboru závislosti oříznut nepoužité závislé součásti.
1. Aplikace a jeho závislostí souboru se umístí do úložiště balíčků modulu runtime. Ke zjištění hostingu při spuštění sestavení a jeho závislosti soubor, budete odkazovat v pár proměnných prostředí.

Odkazy na aplikace konzoly [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) balíčku:

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atribut určuje třídu jako implementace `IHostingStartup` pro spuštění při vytváření a načítání [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). V následujícím příkladu je obor názvů `StartupEnhancement`, a je třída `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

Třída implementuje `IHostingStartup`. Třídy [konfigurovat](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metoda používá [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) přidání vylepšení do aplikace. `IHostingStartup.Configure` v hostitelských spuštění sestavení je volána modulem runtime před `Startup.Configure` v uživatelském kódu, který umožňuje přepsat žádnou konfiguraci poskytované hostujícím sestavení při spuštění uživatelského kódu.

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

Při vytváření `IHostingStartup` závislostí souboru projektu (*\*. deps.json*) nastaví `runtime` umístění sestavení *bin* složky:

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Je zobrazena pouze část souboru. Název sestavení v příkladu je `StartupEnhancement`.

## <a name="specify-the-hosting-startup-assembly"></a>Zadejte hostitelské sestavení po spuštění

Pro třídy knihovny - nebo konzoly aplikace poskytované hostitelem spuštění, zadejte název hostingu při spuštění sestavení `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` proměnné prostředí. Proměnná prostředí je středníkem oddělený seznam sestavení.

Vyhledávají pouze na hostitelských při spuštění sestavení `HostingStartup` atribut. Ukázkové aplikace *HostingStartupApp*, ke zjištění hostování startupy je popsáno výše, je proměnná prostředí je nastavena na následující hodnotu:

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

Hostování sestavení po spuštění můžete nastavit také pomocí [hostování při spuštění sestavení](xref:fundamentals/host/web-host#hosting-startup-assemblies) hostovat nastavení konfigurace.

Při hostování více spuštění sestaví jsou k dispozici, jejich [konfigurovat](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metody jsou provedeny v pořadí, že sestavení jsou uvedeny.

## <a name="activation"></a>Aktivace

Možnosti pro hostování aktivace po spuštění jsou:

* [Úložiště modulu runtime](#runtime-store) &ndash; aktivace nevyžaduje, aby kompilace odkaz pro aktivaci. Ukázková aplikace umístí hostování sestavení po spuštění a soubory závislosti do složky, *nasazení*, které usnadní nasazování hostitelských při spuštění v prostředí multimachine. *Nasazení* složka obsahuje také skript Powershellu, který vytvoří nebo změní proměnné prostředí pro nasazení systému k povolení spuštění hostování.
* Vyžadováno pro aktivaci odkazu za kompilace
  * [Balíček NuGet](#nuget-package)
  * [Složky bin projektu](#project-bin-folder)

### <a name="runtime-store"></a>Úložiště modulu runtime

Hostování implementace spuštění je umístěn v [úložiště modulu runtime](/dotnet/core/deploying/runtime-store). Kompilace odkaz na sestavení není vyžadováno vylepšené aplikace.

Po spuštění hostování sestavení, hostování počáteční soubor projektu slouží jako soubor manifestu pro [dotnet Restore](/dotnet/core/tools/dotnet-store) příkazu.

```console
dotnet store --manifest <PROJECT_FILE> --runtime <RUNTIME_IDENTIFIER>
```

Tímto příkazem umístíte hostování sestavení po spuštění a další závislosti, které nejsou součástí sdílené architektuře v modulu runtime úložišti profilu uživatele na:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

Pokud vyžadujete k umístění sestavení a závislosti pro globální použití, přidejte `-o|--output` umožňuje `dotnet store` příkaz s následující cestou:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

**Upravit a umístěte soubor závislosti spuštění hostování**

Modul runtime umístění je určeno v  *\*. deps.json* souboru. K aktivaci vylepšení, `runtime` element musíte zadat umístění sestavení rozšíření modulu runtime. Předpona `runtime` umístění s `lib/<TARGET_FRAMEWORK_MONIKER>/`:

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

Ve vzorovém kódu (*StartupDiagnostics* projektu), úpravu  *\*. deps.json* souboru se provádí pomocí [Powershellu](/powershell/scripting/powershell-scripting) skriptu. Skript prostředí PowerShell se automaticky aktivuje cíl sestavení v souboru projektu.

Implementaci  *\*. deps.json* soubor musí být na dostupném místě.

Pro použití na uživatele, umístěte ho do *additonalDeps* složku profilu uživatele `.dotnet` nastavení:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

Globální použití, umístěte ho do *additonalDeps* složce instalace .NET Core:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

Sdílené architektuře verze odpovídá verzi modulu sdíleného modulu runtime, který používá cílové aplikace. Sdílený modul runtime se zobrazí v  *\*. runtimeconfig.json* souboru. V ukázkové aplikaci (*HostingStartupApp*), sdílený modul runtime je zadán v *HostingStartupApp.runtimeconfig.json* souboru.

**Seznam spuštění hostování závislostí souboru**

Umístění implementace  *\*. deps.json* souboru je uveden v `DOTNET_ADDITIONAL_DEPS` proměnné prostředí.

Pokud je soubor umístěn v profilu uživatele *.dotnet* složky, nastavte hodnotu proměnné prostředí:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

---

Pokud se soubor nachází v instalaci .NET Core pro globální použití, zadejte úplnou cestu k souboru:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

---

Pro ukázkovou aplikaci (*HostingStartupApp*) k vyhledání souboru závislosti (*HostingStartupApp.runtimeconfig.json*), souboru závislostí je umístěn v profilu uživatele.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Nastavte `DOTNET_ADDITIONAL_DEPS` proměnné prostředí na následující hodnotu:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Nastavte `DOTNET_ADDITIONAL_DEPS` proměnné prostředí na následující hodnotu:

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Nastavte `DOTNET_ADDITIONAL_DEPS` proměnné prostředí na následující hodnotu:

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

---

Příklady toho, jak nastavit proměnné prostředí pro různé operační systémy, najdete v článku [používání více prostředí](xref:fundamentals/environments).

**Nasazení**

Pro usnadnění nasazení hostování při spuštění v prostředí multimachine, vytvoří ukázkovou aplikaci *nasazení* složky v publikované výstup, který obsahuje:

* Hostování při spuštění sestavení.
* Hostování souboru závislostí při spuštění.
* Skript prostředí PowerShell, který vytvoří nebo změní `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` a `DOTNET_ADDITIONAL_DEPS` kvůli podpoře aktivace spuštění hostování. Spusťte skript z příkazového řádku pro správu prostředí PowerShell v systému pro nasazení.

### <a name="nuget-package"></a>Balíček NuGet

Hostování rozšíření spuštění lze zadat balíček NuGet. Balíček nemá `HostingStartup` atribut. Hostování typům spouštění poskytovaný balíček jsou k dispozici do aplikace pomocí jedné z následujících postupů:

* Soubor projektu vylepšené aplikace je odkaz na balíček pro hostování při spuštění v souboru projektu vaší aplikace (kompilace odkaz). Kompilace odkazem na místě, hostování při spuštění sestavení a všechny jeho závislosti jsou začleněny do závislostí souboru aplikace (*\*. deps.json*). Vztahuje se na hostování balíček po spuštění sestavení publikovaná [nuget.org](https://www.nuget.org/).
* Hostování spouštěcí závislosti soubor je k dispozici do vylepšené aplikace, jak je popsáno v [úložiště modulu Runtime](#runtime-store) část (bez kompilace odkaz).

Další informace o balíčcích NuGet a úložiště modulu runtime naleznete v následujících tématech:

* [Vytvoření balíčku NuGet pomocí nástrojů pro různé platformy](/dotnet/core/deploying/creating-nuget-packages)
* [Publikování balíčků](/nuget/create-packages/publish-a-package)
* [Úložiště balíčků modulu runtime](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a>Složky bin projektu

Hostování rozšíření po spuštění můžete zadat *bin*– nasazení sestavení do vylepšené aplikace. Hostování spuštění typy poskytované sestavení jsou k dispozici do aplikace pomocí jedné z následujících postupů:

* Soubor projektu vylepšené aplikace je odkaz na sestavení pro spouštění hostování (kompilace odkaz). Kompilace odkazem na místě, hostování při spuštění sestavení a všechny jeho závislosti jsou začleněny do závislostí souboru aplikace (*\*. deps.json*). Tento postup platí, pokud scénář nasazení volá pro přesun sestavení zkompilované hostování spuštění knihovny (DLL soubor) náročné projektu nebo do umístění přístupné náročné projektem a se kompilace odkazuje na hostování pro spuštění sestavení.
* Hostování spouštěcí závislosti soubor je k dispozici do vylepšené aplikace, jak je popsáno v [úložiště modulu Runtime](#runtime-store) část (bez kompilace odkaz).

## <a name="sample-code"></a>Ukázkový kód

[Ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([stažení](xref:index#how-to-download-a-sample)) ukazuje implementaci scénáře hostingu po spuštění:

* Dvě hostování při spuštění sestavení (knihoven tříd) nastavit pár konfigurace v paměti páry klíč hodnota každé:
  * Balíček NuGet (*HostingStartupPackage*)
  * Knihovna tříd (*HostingStartupLibrary*)
* Hostování spuštění je aktivováno z úložiště nasazení sestavení modulu runtime (*StartupDiagnostics*). Sestavení přidá do aplikace při spuštění obsahující diagnostické informace o dvou middlewares:
  * Registrované služby
  * Adresa (schéma, hostitele, základ cesty, cesta, řetězec dotazu)
  * Připojení (vzdálenou IP adresu, vzdálený port, místní IP adresu, místní port, klientského certifikátu)
  * Hlavičky žádosti
  * Proměnné prostředí

Ke spuštění ukázky:

**Aktivace z balíčku NuGet**

1. Kompilace *HostingStartupPackage* balíček s [balíčku dotnet](/dotnet/core/tools/dotnet-pack) příkazu.
1. Přidat název sestavení daného balíčku *HostingStartupPackage* k `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` proměnné prostředí.
1. Kompilace a spuštění aplikace. Odkaz na balíček je k dispozici vylepšené aplikace (kompilace odkaz). A `<PropertyGroup>` v projektu aplikace soubor Určuje výstup projektu balíček (*... / HostingStartupPackage/bin/Debug*) jako zdroj balíčků. To umožňuje aplikacím používat balíček bez nahrává se balíček, který má [nuget.org](https://www.nuget.org/). Další informace naleznete v poznámkách v souboru projektu HostingStartupApp.

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. Podívejte se, že hodnoty klíče konfigurace služby vykreslen metodou indexovou stránku shodovat s hodnotami nastavenými balíček `ServiceKeyInjection.Configure` metody.

Pokud provedete změny *HostingStartupPackage* projektu a ji znovu zkompilovat, zrušte zaškrtnutí políčka místní mezipaměti balíčku NuGet a zkontrolujte, že *HostingStartupApp* obdrží aktualizovaný balíček a nejsou zastaralé balíček z místní mezipaměti. Pokud chcete vymazat místní mezipaměť NuGet, spusťte následující [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) příkaz:

```console
dotnet nuget locals all --clear
```

**Aktivace z knihovny tříd**

1. Kompilace *HostingStartupLibrary* knihovny tříd pomocí [dotnet sestavení](/dotnet/core/tools/dotnet-build) příkazu.
1. Přidat název sestavení knihovny tříd *HostingStartupLibrary* k `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` proměnné prostředí.
1. *bin*– sestavení knihovny tříd aplikace nasadíte tak, že zkopírujete *HostingStartupLibrary.dll* soubor z knihovny tříd zkompilován výstup do aplikace *bin/Debug* složky.
1. Kompilace a spuštění aplikace. `<ItemGroup>` Soubor v projektu aplikace odkazuje na sestavení knihovny tříd (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (referenční dokumentace kompilace). Další informace naleznete v poznámkách v souboru projektu HostingStartupApp.

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. Podívejte se, že hodnoty klíče konfigurace služby vykreslen metodou indexovou stránku odpovídat hodnoty nastavené v knihovně tříd `ServiceKeyInjection.Configure` metody.

**Aktivace z úložiště nasazení sestavení modulu runtime**

1. *StartupDiagnostics* používá projekt [PowerShell](/powershell/scripting/powershell-scripting) k úpravě jeho *StartupDiagnostics.deps.json* souboru. Prostředí PowerShell se instaluje standardně na Windows od verze Windows 7 SP1 a Windows Server 2008 R2 SP1. Získat PowerShell na jiných platformách, naleznete v tématu [instalace prostředí Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).
1. Sestavení *StartupDiagnostics* projektu. Po projekt je sestaven, cíl sestavení v souboru projektu automaticky:
   * Spustí skript prostředí PowerShell k úpravě *StartupDiagnostics.deps.json* souboru.
   * Přesune *StartupDiagnostics.deps.json* souboru do profilu uživatele *additionalDeps* složky.
1. Spustit `dotnet store` příkaz command prompt v hostitelských spouštěcí adresář k uložení sestavení a jeho závislosti v úložišti profilu uživatele modulu runtime:

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   Pro Windows, tento příkaz používá `win7-x64` [identifikátor modulu runtime (RID)](/dotnet/core/rid-catalog). Při poskytování hostitelských po spuštění pro jiný modul runtime, nahraďte správné identifikátorů RID.
1. Nastavte proměnné prostředí:
   * Přidat název sestavení *StartupDiagnostics* k `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` proměnné prostředí.
   * Na Windows, nastavte `DOTNET_ADDITIONAL_DEPS` proměnnou prostředí, aby `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`. V systému macOS nebo Linuxu, nastavte `DOTNET_ADDITIONAL_DEPS` proměnnou prostředí, aby `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, kde `<USER>` tedy profil, který uživatel, který obsahuje spuštění hostování.
1. Spuštění ukázkové aplikace.
1. Žádosti `/services` koncový bod můžete zobrazit aplikace registrované služby. Žádosti `/diag` koncový bod můžete zobrazit diagnostické informace.
