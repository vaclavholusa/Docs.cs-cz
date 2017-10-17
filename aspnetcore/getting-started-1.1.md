---
title: "Začínáme s ASP.NET Core 1.1"
author: rick-anderson
description: "Rychlý kurz, který vytvoří a spustí jednoduchou aplikaci Hello World pomocí ASP.NET Core 1.1."
keywords: "ASP.NET Core, kurzu Začínáme"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: e8fd9ef60ebc1cff6ca0e03000ea50eebff0a9f9
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core-11"></a>Začínáme s ASP.NET Core 1.1

> [!NOTE]
> Tyto pokyny jsou určené pro ASP.NET Core 1.1. Hledáte nejnovější verzi? V tématu [aktuální verzi v tomto kurzu](xref:getting-started).

1. Instalace .NET Core **instalační program sady SDK** pro sadu SDK 1.0.4 z [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 stáhne stránky](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).

2. Vytvořte složku pro nový projekt .NET Core.

   V systému macOS a Linux otevřete okno terminálu. V systému Windows otevřete příkazový řádek.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. Pokud jste nainstalovali novější verze sady SDK na váš počítač, vytvořte *global.json* soubor a vyberte 1.0.4 SDK.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. Vytvoření nového projektu .NET Core.

   ```terminal
   dotnet new web
   ```
   
3.  Obnovení balíčků.

    ```terminal
    dotnet restore
    ```

4. Spusťte aplikaci.

   `dotnet run` Příkaz sestavení aplikace nejprve v případě potřeby.

   ```terminal
   dotnet run
   ```

5. Přejděte na`http://localhost:5000`

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a>Další kroky

Kurzy Začínáme najdete v části [kurzy jádro ASP.NET](tutorials/index.md)

Úvod do ASP.NET základní koncepty a architektury, najdete v části [ASP.NET Core ÚVOD](index.md) a [ASP.NET Core Základy](fundamentals/index.md).

Aplikace ASP.NET Core pomocí .NET Core nebo základní rozhraní .NET Framework – knihovna tříd a modulu runtime. Další informace najdete v tématu [výběru mezi .NET Core a rozhraní .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
