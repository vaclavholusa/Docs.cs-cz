---
title: Vývoj aplikací ASP.NET Core sledovacích procesů souboru
author: rick-anderson
description: Tento kurz ukazuje, jak nainstalovat a používat nástroje sledovacích procesů (dotnet kukátko) souborů .NET Core CLI v aplikaci ASP.NET Core.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: 2a59267b36faf1e00ea2f0cc7e2b9ceb9828f791
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278850"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a>Vývoj aplikací ASP.NET Core sledovacích procesů souboru

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Victor Hurdugaci](https://twitter.com/victorhurdugaci)

`dotnet watch` je nástroj, který běží [.NET Core rozhraní příkazového řádku](/dotnet/core/tools) příkaz Pokud zdrojové soubory změn. Změna souboru můžete například aktivovat kompilace, spuštění testu nebo nasazení.

Tento kurz používá existující webové rozhraní API s dva koncové body: jeden, který vrátí součet a jeden, který vrací produktu. Metoda produkt obsahuje chyby, která je stanovena v tomto kurzu.

Stažení [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample). Skládá se ze dvou projektů: *WebApp* (ASP.NET Core webového rozhraní API) a *WebAppTests* (testy částí pro webové rozhraní API).

V příkazovém řádku přejděte do *WebApp* složky. Spusťte následující příkaz:

```console
dotnet run
```

Výstup konzoly zobrazí zprávy podobné následujícím (což znamená, že je aplikace spuštěna a čeká na požadavky):

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

Ve webovém prohlížeči, přejděte na `http://localhost:<port number>/api/math/sum?a=4&b=5`. Měli byste vidět výsledek `9`.

Přejděte do produktu rozhraní API (`http://localhost:<port number>/api/math/product?a=4&b=5`). Vrátí `9`, nikoli `20` jako byste očekávali. Tento problém vyřešen později v tomto kurzu.

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a>Přidat `dotnet watch` do projektu

`dotnet watch` Souboru sledovacích procesů nástroj je součástí verze 2.1.300 .NET Core SDK. Následující kroky jsou požadovány při používání dřívější verzi rozhraní .NET Core SDK.

1. Přidat `Microsoft.DotNet.Watcher.Tools` odkaz na balíček *.csproj* souboru:

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. Nainstalujte `Microsoft.DotNet.Watcher.Tools` balíček spuštěním následujícího příkazu:

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a>Spusťte příkazy .NET Core rozhraní příkazového řádku pomocí `dotnet watch`

Všechny [.NET Core rozhraní příkazového řádku příkaz](/dotnet/core/tools#cli-commands) lze spustit s `dotnet watch`. Příklad:

| Příkaz | Příkaz s sledování |
| ---- | ----- |
| Spustit DotNet. | Spustit sledování DotNet. |
| Spustit -f netcoreapp2.0 DotNet. | Spustit -f netcoreapp2.0 sledovat DotNet. |
| Spustit netcoreapp2.0 -f – DotNet – arg1 | kukátko DotNet spustit netcoreapp2.0 -f –--arg1 |
| test DotNet. | test sledovat DotNet. |

Spustit `dotnet watch run` v *WebApp* složky. Určuje, bude výstup konzoly `watch` bylo zahájeno.

## <a name="make-changes-with-dotnet-watch"></a>Proveďte změny s `dotnet watch`

Zajistěte, aby `dotnet watch` běží.

Opravte chyby v `Product` metodu *MathController.cs* tak, aby vracel produktu a nikoli součet:

```csharp
public static int Product(int a, int b)
{
  return a * b;
}
```

Uložte soubor. Výstup konzoly znamená, že `dotnet watch` zjištěna změna souboru a restartování aplikace.

Ověřte `http://localhost:<port number>/api/math/product?a=4&b=5` vrátí výsledek ve správné.

## <a name="run-tests-using-dotnet-watch"></a>Spouštění testů pomocí `dotnet watch`

1. Změna `Product` metodu *MathController.cs* zpět na vrácení součet. Uložte soubor.
1. V příkazovém řádku přejděte do *WebAppTests* složky.
1. Spustit [dotnet obnovení](/dotnet/core/tools/dotnet-restore).
1. Run `dotnet watch test`. Její výstup označuje, že testu se nezdařila a že sledovací proces čeká na změny souboru:

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. Opravte `Product` metoda kód tak, aby vracel produktu. Uložte soubor.

`dotnet watch` zjistí změnu souboru a znovu spustí testy. Výstup konzoly znamená, testy úspěšně.

## <a name="customize-files-list-to-watch"></a>Přizpůsobení seznamu souborů si chcete přehrát

Ve výchozím nastavení `dotnet-watch` sleduje všechny soubory odpovídající následující glob vzorce:

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

Další položky můžete přidat do seznamu sledování úpravou *.csproj* souboru. Položky lze zadat, samostatně nebo pomocí glob vzory.

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a>Výslovný nesouhlas soubory sledovat

`dotnet-watch` může být nakonfigurován pro ignorovat výchozího nastavení. Ignorovat konkrétní soubory, přidejte `Watch="false"` atribut v definici položky *.csproj* souboru:

```xml
<ItemGroup>
    <!-- exclude Generated.cs from dotnet-watch -->
    <Compile Include="Generated.cs" Watch="false" />

    <!-- exclude Strings.resx from dotnet-watch -->
    <EmbeddedResource Include="Strings.resx" Watch="false" />

    <!-- exclude changes in this referenced project -->
    <ProjectReference Include="..\ClassLibrary1\ClassLibrary1.csproj" Watch="false" />
</ItemGroup>
```

## <a name="custom-watch-projects"></a>Vlastní sledování projekty

`dotnet-watch` není omezena na projektů C#. Vlastní sledování projekty mohou být vytvořeny pro zpracování různých scénářů. Vezměte v úvahu následující rozložení projektu:

* **testování nebo**
  * *UnitTests/UnitTests.csproj*
  * *IntegrationTests/IntegrationTests.csproj*

Pokud si chcete přehrát obou projektů je cílem, vytvořte soubor vlastních projektů, který je nakonfigurován tak, aby podívejte se na obou projektů:

```xml
<Project>
    <ItemGroup>
        <TestProjects Include="**\*.csproj" />
        <Watch Include="**\*.cs" />
    </ItemGroup>

    <Target Name="Test">
        <MSBuild Targets="VSTest" Projects="@(TestProjects)" />
    </Target>

    <Import Project="$(MSBuildExtensionsPath)\Microsoft.Common.targets" />
</Project>
```

Chcete-li spustit soubor sledováním v obou projektů, změňte nastavení na *testování* složky. Spusťte následující příkaz:

```console
dotnet watch msbuild /t:Test
```

VSTest provede, když se změní všechny soubory v buď testovacího projektu.

## <a name="dotnet-watch-in-github"></a>`dotnet-watch` v Githubu

`dotnet-watch` je součástí Githubu [DotNetTools úložiště](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).
