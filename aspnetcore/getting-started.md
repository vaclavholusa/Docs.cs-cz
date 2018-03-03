---
title: "Začínáme s ASP.NET Core 2.0"
author: rick-anderson
description: "Rychlý kurz, který vytvoří a spustí jednoduchou aplikaci Hello World pomocí ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: 6f6b580f700a8e2ce09901668e6b026251024619
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="get-started-with-aspnet-core"></a>Začínáme s ASP.NET Core

> [!NOTE]
> Tyto pokyny jsou určeny nejnovější verzi ASP.NET Core. Hledáte začít pracovat se starší verzí? V tématu [1.1 verzi v tomto kurzu](xref:getting-started-1.1).

1. Nainstalujte [.NET Core](https://www.microsoft.com/net/core/).

2. Vytvoření nového projektu .NET Core.

   V systému macOS a Linux otevřete okno terminálu. V systému Windows otevřete příkazový řádek. Zadejte následující příkaz:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. Spusťte aplikaci.

    Ke spuštění aplikace použijte následující příkazy:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. Přejděte do [http://localhost: 5000](http://localhost:5000)

6. Otevřete *Pages/About.cshtml* a upravte stránku a zobrazí se zpráva "Hello, world! Je čas na serveru @DateTime.Now ":

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. Přejděte do [http://localhost: 5000/o](http://localhost:5000/About) a ověřit změny.

### <a name="next-steps"></a>Další kroky

Kurzy Začínáme najdete v části [kurzy jádro ASP.NET](tutorials/index.md)

Úvod do ASP.NET základní koncepty a architektury, najdete v části [ASP.NET Core ÚVOD](index.md) a [ASP.NET Core Základy](fundamentals/index.md).

Aplikace ASP.NET Core pomocí .NET Core nebo základní rozhraní .NET Framework – knihovna tříd a modulu runtime. Další informace najdete v tématu [výběru mezi .NET Core a rozhraní .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
