---
title: Začínáme s ASP.NET Core
author: rick-anderson
description: Rychlý kurz, který vytvoří a spustí jednoduchou aplikaci Hello World pomocí ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: 1b4d765c08c67a65a8752e7f650d4e61ec0d2fbf
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/31/2018
ms.locfileid: "34687454"
---
# <a name="get-started-with-aspnet-core"></a>Začínáme s ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

1. Nainstalujte [!INCLUDE [](~/includes/2.1-SDK.md)].

2. Vytvoření nového projektu .NET Core.

   V systému macOS a Linux otevřete okno terminálu. V systému Windows otevřete příkazový řádek. Zadejte následující příkaz:

    ```terminal
    dotnet new webapp -o aspnetcoreapp
    ```

3. Spusťte aplikaci pomocí následujících příkazů:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Přejděte do [ http://localhost:5000 ](http://localhost:5000).

5. Otevřete *Pages/About.cshtml* a upravte stránku a zobrazí se zpráva "Hello, world! Je čas na serveru @DateTime.Now":

    [!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Přejděte do [ http://localhost:5000/About ](http://localhost:5000/About) a ověřit změny.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. Nainstalujte [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Vytvoření nového projektu .NET Core.

   V systému macOS a Linux otevřete okno terminálu. V systému Windows otevřete příkazový řádek. Zadejte následující příkaz:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. Spusťte aplikaci pomocí následujících příkazů:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Přejděte do [ http://localhost:5000 ](http://localhost:5000).

5. Otevřete *Pages/About.cshtml* a upravte stránku a zobrazí se zpráva "Hello, world! Je čas na serveru @DateTime.Now":

    [!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Přejděte do [ http://localhost:5000/About ](http://localhost:5000/About) a ověřit změny.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Instalace .NET Core **instalační program sady SDK** pro sadu SDK 1.0.4 z [.NET Core všechny soubory ke stažení stránky](https://www.microsoft.com/net/download/all).

2. Vytvořte složku pro nový projekt .NET Core.

   V systému macOS a Linux otevřete okno terminálu. V systému Windows otevřete příkazový řádek.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Pokud jste nainstalovali novější verze sady SDK na váš počítač, vytvořte *global.json* soubor a vyberte 1.0.4 SDK.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Vytvoření nového projektu .NET Core.

   ```terminal
   dotnet new web
   ```

5. Obnovení balíčků.

    ```terminal
    dotnet restore
    ```

6. Spusťte aplikaci.

   ```terminal
   dotnet run
   ```

   [Dotnet spustit](/dotnet/core/tools/dotnet-run) příkaz sestavení aplikace nejprve v případě potřeby.

7. Přejděte do `http://localhost:5000`.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end