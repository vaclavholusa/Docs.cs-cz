---
title: Vývoj aplikací ASP.NET Core sledovacích procesů souboru
author: rick-anderson
description: Tento kurz ukazuje, jak nainstalovat a používat nástroje sledovacích procesů (dotnet kukátko) souborů .NET Core CLI v aplikaci ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 05/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/dotnet-watch
ms.openlocfilehash: 29890640223fe533cca82fb8d39a5ef26e8c6639
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729973"
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

## <a name="dotnet-watch-in-github"></a>kukátko DotNet v Githubu

kukátko DotNet je součástí Githubu [DotNetTools úložiště](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).

[MSBuild části](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) z [dotnet sledovat ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) vysvětluje, jak sledovat dotnet. lze konfigurovat v souboru projektu nástroje MSBuild sledovány. [Dotnet sledovat ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) obsahuje informace o dotnet sledování nejsou zahrnuté v tomto kurzu.
