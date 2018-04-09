---
title: Začínáme s ASP.NET Core 1.1
author: rick-anderson
description: V tomto kurzu rychle vytvořit a spustit jednoduchou aplikaci Hello World pomocí ASP.NET Core 1.1.
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: c61a9a918e51bbd6c1f1142a04473393c8fc54ca
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="get-started-with-aspnet-core-11"></a>Začínáme s ASP.NET Core 1.1

> [!NOTE]
> Tyto pokyny jsou určené pro ASP.NET Core 1.1. Hledáte nejnovější verzi? V tématu [aktuální verzi v tomto kurzu](xref:getting-started).

1. Instalace .NET Core **instalační program sady SDK** pro sadu SDK 1.0.4 z [.NET Core všechny soubory ke stažení stránky](https://www.microsoft.com/net/download/all).

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

   [Dotnet spustit](/dotnet/core/tools/dotnet-run) příkaz sestavení aplikace nejprve v případě potřeby.

   ```terminal
   dotnet run
   ```

5. Přejděte na `http://localhost:5000`

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a>Další kroky

Kurzy Začínáme najdete v části [kurzy jádro ASP.NET](tutorials/index.md)

Úvod do ASP.NET základní koncepty a architektury, najdete v části [ASP.NET Core ÚVOD](index.md) a [ASP.NET Core Základy](fundamentals/index.md).

Aplikace ASP.NET Core pomocí .NET Core nebo základní rozhraní .NET Framework – knihovna tříd a modulu runtime. Další informace najdete v tématu [výběru mezi .NET Core a rozhraní .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
