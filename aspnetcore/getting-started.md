---
title: Začínáme s ASP.NET Core
author: rick-anderson
description: Rychlý kurz, který vytvoří a spustí jednoduchou aplikaci Hello World pomocí ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a>Začínáme s ASP.NET Core

> [!NOTE]
> Tyto pokyny jsou určeny nejnovější verzi ASP.NET Core. V tématu [Začínáme s ASP.NET Core 1.1](xref:getting-started-1.1) 1.1 verze tohoto dokumentu.

1. Nainstalujte [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Vytvoření nového projektu .NET Core.

   V systému macOS a Linux otevřete okno terminálu. V systému Windows otevřete příkazový řádek. Zadejte následující příkaz:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. Spusťte aplikaci.

    Ke spuštění aplikace použijte následující příkazy:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Přejděte na [http://localhost:5000](http://localhost:5000)

5. Otevřete <em>Pages/About.cshtml</em> a upravte stránku a zobrazí se zpráva "Hello, world! Je čas na serveru @DateTime.Now ":

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Přejděte do [ http://localhost:5000/About ](http://localhost:5000/About) a ověřit změny.

### <a name="next-steps"></a>Další kroky

Kurzy Začínáme najdete v části [kurzy jádro ASP.NET](tutorials/index.md)

Úvod do ASP.NET základní koncepty a architektury, najdete v části [ASP.NET Core ÚVOD](index.md) a [ASP.NET Core Základy](fundamentals/index.md).

Aplikace ASP.NET Core pomocí .NET Core nebo základní rozhraní .NET Framework – knihovna tříd a modulu runtime. Další informace najdete v tématu [výběru mezi .NET Core a rozhraní .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
